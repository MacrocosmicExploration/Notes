    scan_code = input("Scan code from FossID: ").strip()

    payload = {
        "group": "scans",
        "action": "get_information",
        "data": {
            "username": username,
            "key": api_key,
            "scan_code": scan_code,
        },
    }




        data = result.get("data")
    print("Data structure:", type(data).__name__)

    if not isinstance(data, dict):
        print("Unexpected scan-detail structure.")
        return

    print("\nAvailable fields:")
    print(", ".join(sorted(data)))

    fields_to_inspect = [
        "id",
        "code",
        "name",
        "project_id",
        "project_code",
        "user_id",
        "username",
        "created",
        "updated",
        "last_run",
        "is_archived",
        "is_from_report",
        "files_not_scanned",
        "pending_items",
    ]

    print("\nSelected scan details:")
    for field in fields_to_inspect:
        value = data[field] if field in data else "[FIELD NOT RETURNED]"
        print(f"{field}: {value}")
