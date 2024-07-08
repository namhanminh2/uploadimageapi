import os
import time
import requests

def upload_image(api_url, image_path, headers=None, params=None, data=None):
    """
    Upload an image to the specified API URL.
    
    :param api_url: The URL of the API to upload the image to.
    :param image_path: The local path to the image to be uploaded.
    :param headers: Any additional headers to include in the request.
    :param params: Any URL parameters to include in the request.
    :param data: Any additional data to include in the request body.
    :return: The response from the API.
    """
    with open(image_path, 'rb') as image_file:
        files = {'image': image_file}
        response = requests.post(api_url, headers=headers, params=params, data=data, files=files)
    return response

def upload_images_in_folder(api_url, folder_path, headers=None, params=None, data=None, delay=5):
    """
    Upload all images in a folder to the specified API URL, with a delay between each upload.
    
    :param api_url: The URL of the API to upload the images to.
    :param folder_path: The local path to the folder containing the images.
    :param headers: Any additional headers to include in the request.
    :param params: Any URL parameters to include in the request.
    :param data: Any additional data to include in the request body.
    :param delay: The delay (in seconds) between each upload.
    """
    successful_uploads = 0  # Đếm số file đã upload thành công
    failed_files = []  # Liệt kê file upload fail
    skipped_files = []  # Liệt kê file bị skip upload
    
    for filename in os.listdir(folder_path):
        if filename.lower().endswith(('.png', '.jpg', '.jpeg', '.gif', '.bmp')):
            image_path = os.path.join(folder_path, filename)
            
            # Check if file has already been uploaded successfully or skipped
            if filename in failed_files or filename in skipped_files:
                print(f'Skipping {filename}: Already processed.')
                continue
            
            response = upload_image(api_url, image_path, headers, params, data)
            if response.status_code == 201:  # Kiểm tra status code 201 (Created)
                successful_uploads += 1
                print(f'Uploaded {filename} - Status Code: {response.status_code}')
                print(f'Response: {response.json()}')  # In response content nếu cần
            elif response.status_code == 200:
                successful_uploads += 1
                print(f'Uploaded {filename} - Status Code: {response.status_code}')
                print(f'Response: {response.json()}')  # In response content nếu cần
            else:
                print(f'Failed to upload {filename} - Status Code: {response.status_code}')
                failed_files.append(filename)  # Thêm file fail vào danh sách
            
            time.sleep(delay)
    
    # Write logs to text files
    write_successful_uploads(successful_uploads)
    write_failed_files(failed_files)
    write_skipped_files(skipped_files)

def write_successful_uploads(successful_uploads):
    """
    Write number of successful uploads to a text file.
    
    :param successful_uploads: Number of successful uploads.
    """
    log_filename = 'successful_uploads.txt'
    with open(log_filename, 'a') as log_file:
        log_file.write(f'Total successful uploads: {successful_uploads}\n')

def write_failed_files(failed_files):
    """
    Write list of failed files to a text file.
    
    :param failed_files: List of filenames that failed to upload.
    """
    log_filename = 'failed_files.txt'
    with open(log_filename, 'a') as log_file:
        log_file.write('Failed uploads:\n')
        for file in failed_files:
            log_file.write(f'- {file}\n')

def write_skipped_files(skipped_files):
    """
    Write list of skipped files to a text file.
    
    :param skipped_files: List of filenames that were skipped.
    """
    log_filename = 'skipped_files.txt'
    with open(log_filename, 'a') as log_file:
        log_file.write('Skipped uploads:\n')
        for file in skipped_files:
            log_file.write(f'- {file}\n')

# Example usage with updated paths
api_url = 'https://api-dev.noah-group.org/api/subject/create-subject-through-image'
folder_path = r'..............'  # Link folder ảnh
headers = {
    'authorization': '...........' # Token
}
data = {
    'majorId': '6675e3004f81df69eccdfa2b',
    'subjectName': 'PRX301',
    'semester': 8,
    'schoolId': '666a8b04ad6b9d3d75f22ead'
}
params = {
    'answer': 'A'  # Updated to be used as a query parameter
}
delay = 10  # delay 10 giây

while True:
    upload_images_in_folder(api_url, folder_path, headers, params, data, delay)
    time.sleep(60)  # Lập lại mỗi (60 giây)
