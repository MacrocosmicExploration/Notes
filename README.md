    payload = {
        "group": "scans",
        "action": "check_status",
        "data": {
            "username": username,
            "key": api_key,
            "scan_code": scan_code,
            "type": "SCAN",
        },
    }


        fields_to_inspect = [
        "scan_id",
        "scan_code",
        "status",
        "state",
        "is_finished",
        "percentage_done",
        "started",
        "finished",
        "total_files",
        "failed_files",
        "ignored_files",
    ]



    
