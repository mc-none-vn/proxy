<div align="center">

# <img src="https://github.com/user-attachments/assets/935a4057-08c7-4414-a628-93e8e2bb6901" height="41"><sup> API Reference </sup><img src="https://github.com/user-attachments/assets/935a4057-08c7-4414-a628-93e8e2bb6901" height="41">

</div>

> [!IMPORTANT]
> Base endpoint is `https://proxy.karasu.io.vn`. All API requests must be routed through this base host. Unauthorized host domains will be rejected.

---

### 📂 1. Get Active Proxy List

Fetch the latest verified, high-speed Socks5 proxy list compiled by the automation pipeline.

*   **Endpoint:** `/list`
*   **Method:** `GET`
*   **Authentication:** `None`

#### 📥 Request Structure
No parameters or headers are required for this endpoint.

#### 📤 Response Structure
*   **Status:** `200 OK`
*   **Content-Type:** `text/plain; charset=utf-8`
*   **Body:** Returns a plain text, newline-separated list of active `IP:Port` combinations.

```text
128.199.65.21:1080
45.77.56.112:4145
198.211.10.254:8080
```

---

### 🔄 2. Execute Request via Proxy Gateway

Forward your HTTP requests through the rotating serverless proxy pool to mask your origin.

*   **Endpoint:** `/v1`
*   **Method:** `GET`
*   **Authentication:** `None`

#### 📥 Request Structure

##### URL Query Parameters
| Parameter | Type     | Position | Required | Description |
| :-------- | :------- | :------- | :------- | :---------- |
| `url`     | `string` | Query    | Optional | The target destination URL to fetch. Must be URL-encoded (e.g., `encodeURIComponent(target)`). |

##### Custom Headers
| Header         | Type     | Required | Description |
| :------------- | :------- | :------- | :---------- |
| `x-target-url` | `string` | Optional | Alternative method to pass the target destination URL. Keeps your request logs cleaner. |

> [!NOTE]
> The gateway prioritization logic parses the `x-target-url` header first. If empty, it falls back to evaluate the `url` query parameter.

#### 📤 Response Structure

##### Success Response
*   **Status:** `200 OK`
*   **Content-Type:** Matches the target server's response type (typically `application/json`).
*   **Body:** Returns the exact payload forwarded from the destination server.

```json
{
  "ip": "185.245.87.12",
  "country": "Germany",
  "proxy_used": "socks5://185.245.87.12:1080"
}
```

##### Error Responses
*   **400 Bad Request** - Triggered when no target URL is provided.
    ```json
    {
      "error": "Bad Request",
      "message": "Target URL is required via 'url' query or 'x-target-url' header."
    }
    ```

*   **403 Forbidden** - Triggered if trying to bypass via unofficial Vercel deployment URLs.
    ```json
    {
      "error": "Forbidden",
      "message": "Direct access to this deployment host is blocked. Please use the official gateway domain."
    }
    ```

*   **502 Bad Gateway** - Triggered if the active proxy node drops the connection or times out (> 3000ms).
    ```json
    {
      "error": "Bad Gateway",
      "message": "Failed to fetch target URL through available proxy nodes. Connection timeout."
    }
    ```
