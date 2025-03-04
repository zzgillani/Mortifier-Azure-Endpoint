# Mortifier Endpoint
This API endpoint acts as a wrapper around the Mortifier CLI tool, allowing for faster and concurrent text extractions when working with large file batches. Given a document's **project ID** and **file GUID**, it eliminates the need to download the document locally prior to running Mortifier.


### Method
    GET http://mortifier-endpoint.azurewebsites.net/api/blob_extraction/{project_id}/{file_guid}
### Parameters
- **project_id**: The GUID of the project, as defined as the container ID of the required file's at-rest Azure Blob storage location.
- **file_guid**: The GUID of the file as defined in the Azure blob.

## Example Python Script
Given a list of file GUIDs, the following Python code block runs and retrieves text extractions concurrently.

    import requests
    import concurrent.futures
    
    # Define list of file GUIDs here
    file_guids = []  # Populate with actual GUIDs
    
    # Specify project ID here
    project_id = "your_project_id"
    
    total_guids = len(file_guids)
    
    # Base URL for the Azure Function
    base_url = f"http://mortifier-endpoint.azurewebsites.net/api/blob_extraction/{project_id}"  
    
    # Define output directory
    output_directory = "your/output/directory/path"
    
    # Error messages from failed GUIDs
    failed_messages = []
    
    def fetch_and_save(i, file_guid):
        """Fetch data from the endpoint for a given GUID and save the response as CSV.
           Returns a tuple (success: bool, error_message: str)."""
        url = f"{base_url}/{file_guid}"
        
        try:
            response = requests.get(url)
            if response.status_code == 200:
                file_path = f'{output_directory}/guid_response_{i}_{file_guid}.csv'
                with open(file_path, 'w', encoding='utf-8') as file:
                    file.write(response.text)
                print(f"Saved CSV for GUID {file_guid} at {file_path}")
                return (True, None)
            else:
                error_message = f"Request {i}/{total_guids} for GUID {file_guid} failed: Status Code {response.status_code}"
                print(error_message)
                return (False, error_message)
        except requests.exceptions.RequestException as e:
            error_message = f"Request {i}/{total_guids} for GUID {file_guid} failed: {e}"
            print(error_message)
            return (False, error_message)
    
    failed_count = 0
    completed_count = 0
    
    with concurrent.futures.ThreadPoolExecutor(max_workers=20) as executor:
        # Submit all tasks, store the futures
        futures = {executor.submit(fetch_and_save, i, row[0]): i for i, row in enumerate(file_guids.itertuples(index=False), start=1)}
        
        for future in concurrent.futures.as_completed(futures):
            completed_count += 1
            success, error_message = future.result()
            if not success:
                failed_count += 1
                failed_messages.append(error_message)
    
    print(f"\nCompleted {total_guids} requests with {failed_count} failures.")
    
    # Log failed GUIDs
    failed_log_file = '{output_directory}/failed_guids_log.txt'
    with open(failed_log_file, 'w', encoding='utf-8') as log_file:
        for message in failed_messages:
            log_file.write(message + "\n")
    
    print(f"Failed messages have been written to {failed_log_file}")

**Note**: When running concurrently, please do not exceed 25 workers. A 20-worker process can handle:
- 1,000 PDFs in ~6 minutes
- 10,000 PDFs in ~30 minutes


