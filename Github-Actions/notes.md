✅ how will u pass artifacts between different jobs and different pipelines

✅  how will u make pipeline faster ? explain with respect to github cache 

✅ how will u invoke manual , push , pull , reusable workflow , cron workflow

✅ explain ci & cd with different pipelines

✅ explain different plugins which u were using and explain how was it connected with github actions

✅ explain sast , dca , dast , unit testing and image vulnerabilty scans with sonarqube

✅ explain dependabots , branch protection rules , codeowner file 

✅ Workflows --> Events --> Jobs --> Steps --> Runners

✅ if one job depends on another how will u do it 

✅ when will use github hosted runner vs self hosted runner

✅ using roles to connect to aws

✅ How do you implement approval before production deployment? explain with respect to environments , required approvers and wait timers 

how will u do blue green deployment using terraform in github action pipeline ? --> use terraform + codedeploy
Secrets leaked in logs. What will you do?

How do you pass data between jobs? explain artifacts and outputs

Do jobs share filesystem?

How do you rollback a failed deployment?

What happens if a step fails? --> By default → job fails --> Can override with continue-on-error: true

Concurrency control

Artifact vs Cache

Workflow optimization

when will u use matrix 

codeql scanning

in security tab , there are 3 things ( dependabots alerts --> get notified when your dependincies has a vulneribility , code scanning alerts --> automatically detect common vulnerabilities and coding errors , secret scanning alerts --> get notified when a secret is pushed to the repository )

there are 2 developers . both are working on 2 different files so there is no conlict . developer 1 pushes code at 5pm & developer 2 pushes code ar 5:01 pm . there is no sense in running the pipeline 2 times as when developer 2 will push the code he needs to do a git pull first . how will u make sure only the pipeline runs for teh 2nd developer ?  --> I would enable branch-level concurrency control in CI/CD so that when a new commit is pushed, any currently running pipeline for that branch is automatically cancelled. This ensures only the latest commit triggers the pipeline, avoiding redundant executions and saving compute cost.


How would you design a secure CI/CD pipeline for production?

How do you handle secret management in pipelines?

How do you implement least privilege access across environments?


How do you reuse workflows across repositories?

How to manage large workflow files efficiently?

What's the difference between public and private workflow repositories?

How to implement workflow concurrency?

How do you handle failed workflows?

How do you reuse workflows across repositories?

How to manage large workflow files efficiently?

What's the difference between public and private workflow repositories?

How to implement workflow concurrency?

How do you handle failed workflows?

