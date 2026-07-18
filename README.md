<div align="center">

# <img src="https://github.com/user-attachments/assets/935a4057-08c7-4414-a628-93e8e2bb6901" height="41"><sup> Automated Serverless Socks5 Proxy Gateway </sup><img src="https://github.com/user-attachments/assets/935a4057-08c7-4414-a628-93e8e2bb6901" height="41">
A production-ready, high-performance HTTP Proxy Function powered by a self-healing serverless architecture. The core routing and benchmarking engine runs entirely on a secure, closed-source automated backend to guarantee connection speeds and operational uptime.

</div>

> [!IMPORTANT]
> Base endpoint is `https://proxy.karasu.io.vn`. All API requests must be routed through this base host. Unauthorized host domains will be rejected.


---

## 🔒 Access & Closed-Source Policy
> [!WARNING]
> **CLOSED INTEGRATION STANDARD:** The structural source code, benchmarking modules (`scanProxy.js`), and cloud infrastructure configuration files are strictly encrypted, obfuscated, and sealed for internal infrastructure routing. 
> 
> *   **Production Consumer Access Only:** This repository functions solely as a gateway interface. End-users are granted authorization exclusively to interact via the exposed public API endpoints.
> *   **Usage Integrity:** Any attempt to scrape raw routing indices, guess endpoint variants, or execute unauthorized resource distribution will trigger immediate edge routing locks.

---

## 🛠️ Key Capabilities

* 🔄 **Dynamic Pool Rotation:** The system auto-rotates target routing profiles behind the scenes every 30 minutes.
* ⚡ **Pre-benched Latency Constraints:** Data routing is dynamically filtered through verified high-speed nodes (under a 3000ms response envelope).
* 🔒 **Wildcard CORS Architecture:** Full support for seamless Cross-Origin Resource Sharing (`CORS`), making it immediately compatible with client-side applications (`fetch`, `axios`).

---

## 📂 Get Active Proxy List

Fetch the latest verified, high-speed Socks5 proxy list compiled by the automation pipeline.

*   **Endpoint:** `/list`
*   **Method:** `GET`
*   **Authentication:** `None`

### 📥 Request Structure
No parameters or headers are required for this endpoint.

### 📤 Response Structure
*   **Status:** `200 OK`
*   **Content-Type:** `text/plain; charset=utf-8`
*   **Body:** Returns a plain text, newline-separated list of active `IP:Port` combinations.

```text
128.199.65.21:1080
45.77.56.112:4145
198.211.10.254:8080
```

---

## 🔄 2. Execute Request via Proxy Gateway

Forward your HTTP requests through the rotating serverless proxy pool to mask your origin.

*   **Endpoint:** `/v1`
*   **Method:** `GET`, `POST`, `PUT`, `DELETE`
*   **Authentication:** `None`

### 📥 Request Structure

#### URL Query Parameters
| Parameter | Type     | Position | Required | Description |
| :-------- | :------- | :------- | :------- | :---------- |
| `url`     | `string` | Query    | Optional | The target destination URL to fetch. Must be URL-encoded (e.g., `encodeURIComponent(target)`). |

#### Custom Headers
| Header         | Type     | Required | Description |
| :------------- | :------- | :------- | :---------- |
| `x-target-url` | `string` | Optional | Alternative method to pass the target destination URL. Keeps your request logs cleaner. |

> [!NOTE]
> The gateway prioritization logic evaluates the `url` query string parameter first. If it is omitted or empty, the controller falls back to evaluate the custom header `x-target-url`.

### 📤 Response Structure

#### Success Response
*   **Status:** Inherits the specific response status code issued by the downstream server (`200 OK`, `201 Created`, etc.).
*   **Content-Type:** Matches target delivery parameters (Fully supports transmission of image streams, file blobs, JSON payloads, and audio raw buffers).
*   **Body:** Returns the exact payload forwarded from the destination server.

```json
{
  "ip": "185.245.87.12",
  "country": "Germany"
}
```

<details><summary>Error Responses</summary>
  <details><summary><b>400 Bad Request</b></summary>

  > Triggered when no target destination parameter is present inside the parsing scope.
  > ```json
  > {
  >   "error": "Missing 'url' parameter specifying the target endpoint."
  > }
  > ```
  </details>
  <details><summary><b>403 Forbidden</b></summary>

  > Triggered if trying to bypass via unofficial deployment host URLs.
  > ```json
  > {
  >   "error": "Forbidden",
  >   "message": "Direct access to this deployment host is blocked. Please use the official gateway domain."
  > }
  > ```
  </details>
  <details><summary><b>500 Internal Server Error (No Proxies)</b></summary>

  > Emitted if backend environmental resources fail to load fallback indices.
  > ```json
  > {
  >   "error": "No proxies found from Vercel env or GitHub fallback."
  > }
  > ```
  </details>
  <details><summary><b>500 Vercel Proxy Error</b></summary>

  > Emitted when proxy node communication completely drops or connection limits hit execution bounds (> 8000ms).
  > ```json
  > {
  >   "error": "Vercel Proxy Error",
  >   "message": "connect ETIMEDOUT 45.77.56.112:4145"
  > }
  > ```
  </details>
</details>

---

## 📄 License & Terms
This system interface is provided under standard consumption conditions.
> [!CAUTION]
> **End-User Disclaimer:** The automated proxy pool relies heavily on public global node metrics. System performance metrics, cryptographic handshakes, and route continuity are provided without warranties. Developers are advised to apply localized security layers on top of target authentication headers.
