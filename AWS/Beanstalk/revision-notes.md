
# AWS Elastic Beanstalk - Revision Notes (10+ Years Experience)

## Core Concepts & Architecture

### What is Elastic Beanstalk?
- **PaaS offering** that orchestrates AWS services (EC2, ELB, Auto Scaling, RDS, CloudWatch)
- You manage the **application code**, AWS manages the **infrastructure**
- Free service - you only pay for underlying AWS resources
- Supports blue/green deployments, rolling updates, and immutable deployments

### Key Components
1. **Application** - Logical container for all Beanstalk components
2. **Application Version** - Specific labeled iteration of deployable code (stored in S3)
3. **Environment** - Collection of AWS resources running an application version
4. **Environment Tier** - Web Server vs Worker tier
5. **Environment Configuration** - Settings that define environment behavior
6. **Saved Configuration** - Template for creating environments

### Architecture Tiers
- **Web Server Tier**: ELB → EC2 instances → RDS (optional)
- **Worker Tier**: SQS queue → EC2 instances processing messages (daemon manages queue)

---

## Supported Platforms

### Languages & Frameworks
- **Docker** (single/multi-container, preconfigured)
- **Go**
- **.NET (Core & Framework)** on Windows/Linux
- **Java** (SE, Tomcat)
- **Node.js**
- **PHP**
- **Python**
- **Ruby** (Passenger, Puma)

### Platform Lifecycle
- **Managed updates** available (security patches, platform updates)
- Can configure maintenance windows
- Platform versions follow semantic versioning
- Old platforms eventually deprecated (plan migrations)

---

## Deployment Strategies

### All at Once
- Deploys to all instances simultaneously
- **Downtime**: Yes (brief)
- **Fastest** deployment method
- Use for: dev/test environments

### Rolling
- Deploys in batches (configurable batch size)
- **Downtime**: No
- Reduced capacity during deployment
- Can configure batch size (%, count)
- Use for: staging, pre-prod

### Rolling with Additional Batch
- Launches new batch first, then rolling deployment
- **Downtime**: No
- **Full capacity** maintained
- Slightly longer deployment time
- Use for: production (cost-conscious)

### Immutable
- Launches full set of new instances in separate ASG
- **Downtime**: No
- **Full capacity** maintained + additional instances
- Safest rollback (terminate new ASG)
- Longest deployment time
- Use for: critical production environments

### Blue/Green
- Creates entirely new environment
- **Zero downtime** with DNS swap
- Full environment testing before swap
- Easy rollback (swap back)
- **Costliest** (2x resources during transition)
- Use for: major version changes, mission-critical apps

### Traffic Splitting (Canary)
- Routes percentage of traffic to new version
- Monitors metrics before full deployment
- Automated rollback on alarm
- Use for: A/B testing, gradual rollouts

---

## Configuration Management

### Configuration Precedence (Highest to Lowest)
1. Settings applied directly via console/CLI/API
2. Saved configurations
3. `.ebextensions` config files
4. Default values

### .ebextensions
- Location: `.ebextensions/` directory in source bundle
- Format: YAML or JSON
- Naming: `*.config` (alphabetically processed)
- Capabilities:
  - Install packages (yum, rpm, rubygems, python, etc.)
  - Create files
  - Run commands (pre-deploy, post-deploy)
  - Configure services
  - Set environment properties
  - Modify container settings

**Example Structure:**
```yaml
# .ebextensions/01_packages.config
packages:
  yum:
    gcc: []
    git: []

commands:
  01_some_command:
    command: "echo 'hello world'"
    cwd: /home/ec2-user

files:
  "/etc/myapp/config.json":
    mode: "000644"
    owner: root
    group: root
    content: |
      { "key": "value" }

container_commands:
  01_migrate:
    command: "python manage.py migrate"
    leader_only: true
```

### Environment Properties
- Access via environment variables
- Visible to application code
- Can store secrets (but use Secrets Manager for production)
- Set via console, CLI, or `.ebextensions`

### Saved Configurations
- Reusable templates for environment settings
- JSON format stored in S3
- Can be applied to new/existing environments
- Useful for disaster recovery

---

## Scaling & Load Balancing

### Auto Scaling Configuration
- **Metrics**: CPU, Network, Request count, Latency, or custom CloudWatch metrics
- **Scaling triggers**: Define min/max instances, thresholds, cooldown periods
- **Time-based scaling**: Schedule scaling actions
- **Instance types**: Can't mix in single environment (use separate envs)

### Load Balancer Options
1. **Classic Load Balancer (CLB)** - Legacy, being phased out
2. **Application Load Balancer (ALB)** - HTTP/HTTPS, path-based routing, WebSocket
3. **Network Load Balancer (NLB)** - TCP/UDP, ultra-low latency, static IPs

**ALB Features:**
- Host-based routing
- Path-based routing  
- HTTP/2 and WebSocket support
- Access logs to S3
- Health check configuration

### Health Checks
- **Basic health check**: Ping path, timeout, interval
- **Enhanced health reporting**: System metrics, deployment monitoring
- Health check grace period (don't mark unhealthy during startup)

---

## Monitoring & Logging

### CloudWatch Integration
- Default metrics: CPU, Network, Disk, Request count, Latency, HTTP status codes
- Custom metrics via CloudWatch agent
- Set alarms for auto-scaling or notifications
- Enhanced health reporting (additional instance/environment metrics)

### Log Access
- **Log streaming**: Real-time logs to CloudWatch Logs
- **Request logs**: Can enable for ALB/CLB
- **Bundle logs**: Download last 100 lines or full logs via console/CLI
- **Tail logs**: Real-time log viewing via EB CLI (`eb logs -f`)

**Log Locations (Linux):**
- `/var/log/eb-engine.log` - Deployment logs
- `/var/log/eb-activity.log` - Activity logs
- `/var/log/nginx/access.log` - Web server access
- `/var/log/nginx/error.log` - Web server errors
- Application-specific logs in `/var/log/`

### X-Ray Integration
- Enable distributed tracing
- Requires X-Ray daemon configuration
- Use for microservices debugging

---

## Database Integration

### RDS Integration
- **Decoupled** (recommended for production): Create RDS separately, connect via env variables
- **Coupled**: Create via Beanstalk console (lifecycle tied to environment - **avoid for production**)

**Best Practice:**
1. Create RDS instance independently
2. Store connection string in Secrets Manager
3. Reference in environment properties
4. Configure security groups for EB ↔ RDS communication

### ElastiCache Integration
- Not directly integrated
- Create separately, configure via environment properties
- Ensure security group rules allow access

---

## Security

### IAM Roles
- **Service Role**: Allows Beanstalk to manage AWS resources on your behalf
- **Instance Profile**: Role assumed by EC2 instances (app uses this for AWS API calls)

### Network Security
- Deploy in VPC (public/private subnets)
- Configure security groups (EB creates defaults)
- Use NAT Gateway for private subnet internet access
- Application Load Balancer can be internal or internet-facing

### HTTPS/SSL
- Upload SSL certificate to AWS Certificate Manager (ACM) or IAM
- Configure via Load Balancer settings
- Enforce HTTPS via `.ebextensions` (redirect HTTP → HTTPS)
- End-to-end encryption: ALB → EC2 (configure via `.ebextensions`)

### Managed Platform Updates
- Security patches applied automatically (configurable)
- Configure maintenance window
- Can defer updates (risky)

---

## Docker Support

### Single Container Docker
- Uses `Dockerfile` or `Dockerrun.aws.json` (v1)
- Simplest Docker deployment
- One container per EC2 instance

### Multi-Container Docker
- Uses `Dockerrun.aws.json` (v2) - similar to ECS task definition
- Requires ECS integration (Beanstalk creates ECS cluster)
- Multiple containers per instance
- Supports sidecar containers (logging, monitoring)

**Dockerrun.aws.json v2 Example:**
```json
{
  "AWSEBDockerrunVersion": 2,
  "containerDefinitions": [
    {
      "name": "app",
      "image": "myapp:latest",
      "memory": 512,
      "portMappings": [{"hostPort": 80, "containerPort": 8080}]
    },
    {
      "name": "nginx",
      "image": "nginx:latest",
      "memory": 128,
      "links": ["app"]
    }
  ]
}
```

### Preconfigured Docker Platforms
- Pre-built containers (Glassfish, Python, Go, etc.)
- Less flexible than custom Docker

---

## Worker Tier (SQS Processing)

### Architecture
- SQS queue created automatically
- Daemon (`sqsd`) polls queue, POSTs messages to local HTTP path
- Periodic tasks via `cron.yaml`
- Dead letter queue for failed messages

### Configuration
- **HTTP path**: Where messages are POSTed (default: `/`)
- **MIME type**: `application/json` or custom
- **HTTP connections**: Concurrent request limit
- **Visibility timeout**: Should exceed task processing time
- **Error visibility timeout**: Retry delay

### cron.yaml (Periodic Tasks)
```yaml
version: 1
cron:
  - name: "task1"
    url: "/scheduled-task"
    schedule: "0 */12 * * *"  # Every 12 hours
```

### Use Cases
- Video/image processing
- Email sending
- Report generation
- Long-running background jobs

---

## CI/CD Integration

### EB CLI
```bash
# Initialize
eb init -p python-3.9 my-app --region us-east-1

# Create environment
eb create prod-env --database.engine postgres

# Deploy
eb deploy

# Logs
eb logs --stream

# SSH
eb ssh

# Environment info
eb status

# Terminate
eb terminate prod-env
```

### CodePipeline Integration
1. Source: CodeCommit/GitHub/S3
2. Build: CodeBuild (run tests, create artifact)
3. Deploy: Elastic Beanstalk

### GitHub Actions / Jenkins
- Use AWS CLI or EB CLI in pipeline
- Store credentials in secrets
- Create application version → Deploy to environment

---

## Advanced Topics

### Custom AMI
- Bake dependencies into AMI (faster deployments)
- Update platform version → may need new AMI
- Trade-off: Less flexibility vs faster deployments

### Custom Platform (Packer)
- Build your own platform from scratch
- Full control over OS, runtime, configs
- Maintain using Packer
- Complex, rarely needed

### Environment Cloning
- Clone entire environment configuration
- Useful for blue/green deployments
- Creates new environment with identical settings

### Swap Environment URLs (CNAME Swap)
- Zero-downtime blue/green deployment
- Swap DNS CNAMEs between environments
- Can take few minutes to propagate
- Use for major version changes

### Resource Lifecycle
- **Resources created by EB**: Deleted when environment terminates
- **Resources created in `.ebextensions`**: May persist (depends on `DeletionPolicy`)
- RDS coupled to environment: **Deleted** on termination (use snapshots!)

---

## Best Practices

### Production Checklist
✅ Use **immutable** or **blue/green** deployments  
✅ Enable **enhanced health reporting**  
✅ Configure **CloudWatch alarms** (CPU, latency, 5xx errors)  
✅ Enable **log streaming** to CloudWatch Logs  
✅ Use **external RDS** (not coupled to environment)  
✅ Store secrets in **Secrets Manager** or **Parameter Store**  
✅ Configure **auto-scaling** based on application metrics  
✅ Use **Application Load Balancer** (not Classic)  
✅ Enable **HTTPS** with ACM certificates  
✅ Deploy in **VPC** with proper subnet design  
✅ Configure **backup/DR strategy** (saved configurations, RDS snapshots)  
✅ Set up **multi-AZ** for HA  
✅ Use **tagging** for cost allocation  
✅ Implement **health check endpoint** in application  
✅ Document `.ebextensions` configurations

### Cost Optimization
- Right-size instance types
- Use Savings Plans or Reserved Instances
- Terminate unused environments
- Schedule scaling (scale down during off-hours)
- Use spot instances for dev/test (via `.ebextensions`)

### Performance
- Use CloudFront for static content
- Enable **connection draining** (drain existing connections before terminating)
- Configure **cross-zone load balancing**
- Optimize health check intervals
- Use appropriate instance types (compute/memory optimized)

### Disaster Recovery
- Regular **saved configuration** exports
- **RDS snapshots** (automated + manual)
- Store **application versions** in S3 with versioning
- **Multi-region** setup for critical applications
- Document **runbooks** for common failures

---

## Troubleshooting

### Common Issues

**Deployment Failures**
- Check `/var/log/eb-engine.log` and `/var/log/eb-activity.log`
- Verify `.ebextensions` syntax (YAML/JSON)
- Check container_commands (use `leader_only: true` for migrations)
- Review CloudFormation events (EB uses CFN under the hood)

**Health Check Failures**
- Verify application responds on health check path
- Check security group rules (ALB → instances)
- Increase health check grace period
- Review application logs for startup errors

**Connection Issues**
- Security group rules (outbound from EB, inbound to RDS)
- VPC routing (NAT Gateway, Internet Gateway)
- Database connection string (check env variables)
- Database security group (allow from EB security group)

**High Latency**
- Check CloudWatch metrics (CPU, network)
- Review slow query logs (RDS)
- Enable X-Ray for distributed tracing
- Optimize application code
- Scale horizontally (increase instance count)

**Environment Updates Stuck**
- CloudFormation stack likely stuck
- Check CFN events in console
- May need AWS support to manually intervene
- Worst case: clone environment, terminate stuck one

### Debugging Commands
```bash
# SSH into instance
eb ssh

# Stream logs
eb logs --stream

# Check environment health
eb health

# List environments
eb list

# Environment status
eb status

# Console URL
eb console

# Local testing
eb local run
```

---

## Limits & Quotas (Per Region)

- **Applications**: 75
- **Application Versions**: 1,000 per application
- **Environments**: 200 (can request increase)
- **Configuration Templates**: 2,000 per application
- **Environment characters in name**: 40
- **Application version size**: 512 MB

---

## Migration Strategies

### From EC2/On-Prem to Beanstalk
1. Containerize application (Docker) or match platform
2. Create `.ebextensions` for dependencies
3. Test in dev environment
4. Blue/green cutover

### From Beanstalk to ECS/EKS
1. Export Docker images
2. Create ECS task definitions / Kubernetes manifests
3. Migrate RDS connections
4. Update DNS

### Between Regions
1. Create saved configuration
2. Copy application versions to target region S3
3. Create environment in new region
4. Update Route 53 for failover/geolocation routing

---

## Interview/Certification Focus

**Key Differentiators:**
- When to use Beanstalk vs ECS vs EKS vs Lambda
- Deployment strategies (which for what scenario)
- Worker tier vs Lambda for background jobs
- RDS coupling (when to avoid)
- Platform vs Docker vs Custom Platform

**Scenario Questions:**
- Zero-downtime deployments → Immutable or Blue/Green
- Cost optimization → Rolling updates, right-sizing
- Quick rollback needed → Immutable (terminate new ASG)
- Background job processing → Worker tier
- Multi-container microservices → Multi-container Docker (or consider ECS)

---

## Quick Reference

### Key CLI Commands
```bash
eb init                    # Initialize EB project
eb create <env-name>       # Create environment
eb deploy                  # Deploy application
eb terminate <env-name>    # Terminate environment
eb swap <env1> <env2>      # CNAME swap
eb clone <env>             # Clone environment
eb config                  # Edit configuration
eb setenv KEY=VALUE        # Set environment variable
eb printenv                # Show environment variables
eb open                    # Open app in browser
```

### Important Environment Variables
- `AWS_REGION` - AWS region
- `RDS_HOSTNAME`, `RDS_PORT`, `RDS_DB_NAME`, `RDS_USERNAME`, `RDS_PASSWORD` - RDS (if coupled)
- Custom variables defined in configuration

---

## When NOT to Use Elastic Beanstalk

❌ Need fine-grained control over infrastructure  
❌ Complex microservices architecture (consider ECS/EKS)  
❌ Serverless requirements (use Lambda)  
❌ Windows-specific enterprise applications (use EC2)  
❌ Frequent infrastructure changes (IaC with Terraform/CloudFormation better)  
❌ Multi-tenant SaaS with environment per customer (cost prohibitive)

---

## Summary

**Elastic Beanstalk is ideal for:**
- Traditional web applications
- Developers who want to focus on code, not infrastructure
- Quick prototyping and MVPs
- Standardized deployment patterns
- Teams without dedicated DevOps

**Remember:** Beanstalk abstracts complexity but you're still responsible for application architecture, database design, security, monitoring, and cost optimization.
