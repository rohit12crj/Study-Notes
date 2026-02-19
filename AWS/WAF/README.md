4XX – Client Side Errors
🔴 400 – Bad Request

➡ Malformed request (invalid JSON, headers missing)

🔐 401 – Unauthorized

➡ Authentication failed (invalid/expired token)

⛔ 403 – Forbidden

➡ Access blocked

Very common in WAF (rule triggered, geo block, IP block)

❓ 404 – Not Found

➡ Wrong URL / route not configured

🚦 405 – Method Not Allowed

➡ HTTP method not supported (POST vs GET mismatch)

🚫 408 – Request Timeout

➡ Client didn’t send request in time

🚨 429 – Too Many Requests

➡ Rate limit exceeded (WAF or API throttling)


5XX – Server Side Errors
💥 500 – Internal Server Error

➡ Application crash / unhandled exception

🔄 502 – Bad Gateway

➡ Backend returned invalid response (ALB ↔ app issue)

🛑 503 – Service Unavailable

➡ No healthy targets / overloaded system

⏳ 504 – Gateway Timeout

➡ Backend too slow / timeout exceeded

How do you differentiate between WAF 403 vs Backend 403?
