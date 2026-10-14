import getpass
import json

import requests


def main():
    api_url = input(
        "Production API URL (https://your-server/api.php): "
    ).strip()

    if not api_url.startswith("https://"):
        print("Use the production HTTPS API address.")
        return

    username = input("WebApp API Username: ").strip()
    api_key = getpass.getpass("WebApp API key (input hidden): ").strip()

    payload = {
        "group": "projects",
        "action": "list_projects",
        "data": {
            "username": username,
            "key": api_key,
        },
    }

    try:
        response = requests.post(
            api_url,
            json=payload,
            timeout=(10, 60),
            allow_redirects=False,
        )
    except requests.exceptions.SSLError:
        print("Certificate verification failed. Keep verification enabled.")
        print("We may need your company's CA certificate configuration.")
        return
    except requests.exceptions.RequestException as exc:
        print(f"Connection failed: {type(exc).__name__}")
        return

    print(f"\nHTTP status: {response.status_code}")

    if 300 <= response.status_code < 400:
        print("The address redirected. Verify the direct API endpoint.")
        return

    if response.status_code != 200:
        print("The server did not return HTTP 200.")
        return

    try:
        result = response.json()
    except ValueError:
        print("Response was not JSON; it may be a login or proxy page.")
        return

    if not isinstance(result, dict):
        print("Unexpected response structure:", type(result).__name__)
        return

    print("Operation:", result.get("operation"))
    print("API status:", result.get("status"))

    if str(result.get("status")) != "1":
        for field in ("message", "error"):
            text = json.dumps(result.get(field), ensure_ascii=False)
            if api_key:
                text = text.replace(api_key, "[REDACTED]")
            print(f"{field}: {text}")
        return

    data = result.get("data")
    print("Data structure:", type(data).__name__)

    if isinstance(data, list):
        print("Project records returned:", len(data))
        if data and isinstance(data[0], dict):
            print("Available fields:", ", ".join(sorted(data[0])))
        print("\nFirst five projects — review locally:")
        for project in data[:5]:
            if isinstance(project, dict):
                print(
                    project.get("id"),
                    project.get("project_code"),
                    project.get("project_name"),
                )
    elif isinstance(data, dict):
        print("Data keys:", ", ".join(sorted(data)))
    else:
        print("No project list returned.")


if __name__ == "__main__":
    main()
