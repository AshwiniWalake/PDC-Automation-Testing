# PDC-Automation-Testing

from appium import webdriver
from appium.webdriver.common.appiumby import AppiumBy
import time

# Set desired capabilities
desired_caps = {
    "platformName": "Android",
    "platformVersion": "11",  # Change to your emulator/device version
    "deviceName": "Android Emulator",  # or actual device name
    "app": "/path/to/your.apk",  # Path to the downloaded APK
    "automationName": "UiAutomator2",
    "autoGrantPermissions": True
}

# Start Appium driver
driver = webdriver.Remote("http://localhost:4723/wd/hub", desired_caps)
driver.implicitly_wait(10)  # Wait up to 10 seconds for elements to appear

def create_record(i):
    # Example: Fill form fields (change resource IDs/names as per your app)
    driver.find_element(AppiumBy.ID, "com.example:id/input_field1").send_keys(f"Test {i}")
    driver.find_element(AppiumBy.ID, "com.example:id/input_field2").send_keys(f"Value {i}")
    # ...add more fields as needed
    driver.find_element(AppiumBy.ID, "com.example:id/add_button").click()  # e.g., "Add" button

try:
    for i in range(1, 51):  # Create 50 records
        create_record(i)
        time.sleep(0.5)  # Adjust sleep as needed for animations

    # After 50 records, trigger the Submit CTA (change resource ID)
    driver.find_element(AppiumBy.ID, "com.example:id/submit_button").click()
    print("Submitted successfully after creating 50 records.")

    # Optionally, add assertions to verify successful submission
    # success_msg = driver.find_element(AppiumBy.ID, "com.example:id/success_message").text
    # assert "Success" in success_msg

finally:
    driver.quit()
