## Part A: User Manual Procedure

### Creating and Activating a Python Virtual Environment & Installing a Package

#### Prerequisites

Before beginning this tutorial, ensure you have:

* A computer running Windows, macOS, or Linux with basic internet access.
* Python 3.8 or higher installed on your system.
* Access to a terminal application (**Command Prompt** or **PowerShell** on Windows; **Terminal** on macOS/Linux).

---

#### Step-by-Step Procedure

1. **Open your terminal application.**
* **Action:** Open Command Prompt / PowerShell on Windows, or Terminal on macOS/Linux.
* **Expected Result:** A terminal window appears showing a command prompt prompt line ending with `>` or `$`.


2. **Create a project folder and navigate inside it.**
* **Action:** Run the following commands:
```bash
mkdir my_python_project
cd my_python_project

```


* **Expected Result:** Your working directory changes to `my_python_project`.


3. **Create the virtual environment.**
* **Action:** Run the command for your operating system:
* **Windows:** `python -m venv venv`
* **macOS/Linux:** `python3 -m venv venv`


* **Expected Result:** A new directory named `venv` is generated inside your project folder containing a isolated Python installation.


4. **Activate the virtual environment.**
* **Action:** Run the activation command for your operating system:
* **Windows (Command Prompt):** `venv\Scripts\activate`
* **Windows (PowerShell):** `.\venv\Scripts\Activate.ps1`
* **macOS/Linux:** `source venv/bin/activate`


* **Expected Result:** Your terminal prompt prefix changes to include `(venv)`, indicating that the virtual environment is currently active.


5. **Install a package using pip.**
* **Action:** Run `pip install requests`.
* **Expected Result:** Terminal output displays download progress bars followed by a success message such as `Successfully installed requests-...`.


6. **Verify the installation.**
* **Action:** Run `pip list`.
* **Expected Result:** A table of installed packages prints to the console showing `requests` alongside its dependencies (`urllib3`, `certifi`, etc.).



---

#### Screenshot Placeholder

> **[Screenshot: Terminal with Active Virtual Environment]**
> *Description:* A screenshot of the terminal window after completing Step 5. The top line shows the command `pip install requests` being executed, followed by pip's download status bars and installation confirmation lines. The prompt at the bottom displays `(venv) C:\Users\Student\my_python_project>`, highlighting the `(venv)` indicator on the far left.

---

#### Troubleshooting

* **Error:** `Execution of scripts is disabled on this system` (Windows PowerShell users).
* **Cause:** Windows PowerShell restricts running external scripts by default under its execution policy.
* **Solution:** Open PowerShell as an Administrator and run `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`, then press `Y` to confirm. Close the Administrator window and re-attempt Step 4 in your regular terminal.

---

## Part B: API Reference Entry

### Create Task

Creates a new task within a specified project for the authenticated user.

* **HTTP Method:** `POST`
* **Endpoint Path:** `/api/v1/projects/{project_id}/tasks`

---

### Request Headers

| Header Name | Type | Required | Description |
| --- | --- | --- | --- |
| `Authorization` | String | Required | Bearer token formatted as `Bearer <your_api_token>`. |
| `Content-Type` | String | Required | Must be set to `application/json`. |

---

### Request Parameters

#### Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `project_id` | String | Required | The unique UUID identifier of the project where the task will be created. |

#### Body Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `title` | String | Required | Concise summary of the task (1–255 characters). |
| `description` | String | Optional | Detailed description or context for the task. Default is `null`. |
| `assignee_id` | String | Optional | Unique UUID of the user assigned to the task. Default is `null`. |
| `due_date` | String | Optional | Task due date formatted as an ISO 8601 string (`YYYY-MM-DD`). Default is `null`. |
| `priority` | String | Required | Task priority level. Allowed values: `"low"`, `"medium"`, `"high"`. |

---

### Example Request Body

```json
{
  "title": "Setup CI/CD Pipeline",
  "description": "Configure GitHub Actions workflow for automated testing and staging deployments.",
  "assignee_id": "usr_9f8e7d6c-5b4a-3f2e-1d0c-9b8a7f6e5d4c",
  "due_date": "2026-10-15",
  "priority": "high"
}

```

---

### HTTP Response Codes

| Status Code | Reason | Description |
| --- | --- | --- |
| `201 Created` | Created | The task was successfully created. Returns the created task payload. |
| `400 Bad Request` | Invalid Input | Missing required fields, invalid enum values (e.g., priority not low/medium/high), or malformed JSON body. |
| `401 Unauthorized` | Authentication Failed | Authorization header is missing, invalid, or expired. |
| `404 Not Found` | Resource Not Found | Target `project_id` or provided `assignee_id` does not exist. |
| `500 Internal Server Error` | Server Error | An unexpected error occurred on the server while processing the request. |

---

### Example Successful Response (`201 Created`)

```json
{
  "status": "success",
  "data": {
    "id": "tsk_1a2b3c4d-5e6f-7a8b-9c0d-1e2f3a4b5c6d",
    "project_id": "prj_88f3a91b-2c4d-4e5f-9a0b-1c2d3e4f5a6b",
    "title": "Setup CI/CD Pipeline",
    "description": "Configure GitHub Actions workflow for automated testing and staging deployments.",
    "status": "todo",
    "priority": "high",
    "assignee": {
      "id": "usr_9f8e7d6c-5b4a-3f2e-1d0c-9b8a7f6e5d4c",
      "name": "Jane Doe",
      "email": "jane.doe@example.com"
    },
    "created_by": "usr_3c2b1a0f-9e8d-7c6b-5a4f-3e2d1c0b9a8f",
    "due_date": "2026-10-15",
    "created_at": "2026-10-03T14:30:00Z",
    "updated_at": "2026-10-03T14:30:00Z"
  }
}

```