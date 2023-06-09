# freestuff
<details>
<summary>Automate Log Into Appian From Ubuntu 22.04 </summary>

```Python
# -----------------------------------------------------
# Install Google Chrome
# -----------------------------------------------------
# 1) Check if Google Chrome is installed using
#   google-chrome-stable --version
# If not installed, follow the instructions to install Google Chrome Into Ubuntu
# 2) Update the package lists and upgrade existing packages by running the following commands:
#   sudo apt update
#   sudo apt upgrade -y
# 3) Install the dependencies required for Google Chrome installation:
#   sudo apt install -y wget curl unzip
# 4) Download the Google Chrome installation package for Ubuntu using wget:
#   wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
# 5) Install the downloaded package using dpkg:
#   sudo dpkg -i google-chrome-stable_current_amd64.deb
# 6) If there are any missing dependencies, you may need to run the following command to fix them:
#   sudo apt --fix-broken install -y
# 7) Verify that Google Chrome is installed by running the following command:
#   google-chrome-stable --version

# -----------------------------------------------------
# Configure SystemD to run this python script at midnight every day
# -----------------------------------------------------
# 1) Create a file called autoLogIntoAppianEveryday.service with the following content:
# [Unit]
# Description=Run autoLogIntoAppianEveryday script

# [Service]
# User=ubuntu
# ExecStart=/usr/bin/python3 /home/ubuntu/python/logIntoAppian.py
# WorkingDirectory=/home/ubuntu/python
# Restart=on-failure

# [Install]
# WantedBy=multi-user.target
#
# 2) Make sure autoLogIntoAppianEveryday.service is moved to the directory: /etc/systemd/system/
#   sudo mv autoLogIntoAppianEveryday.service /etc/systemd/system/
# 3) Reload the systemd configuration to apply the changes
#   sudo systemctl daemon-reload
# 4) Enable the timer to start automatically at boot:
#   sudo systemctl enable autoLogIntoAppianEveryday.service
# 5) Start the service:
#   sudo systemctl start autoLogIntoAppianEveryday.service
# 6) Verify that the service is running:
#   sudo systemctl status autoLogIntoAppianEveryday.service

# See Steps to create a timer to trigger the above service 
    
from selenium import webdriver
from selenium.webdriver.chrome.service import Service as ChromeService
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

# Create Chrome options and add --no-sandbox and --disable-dev-shm-usage options
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument('--no-sandbox')
chrome_options.add_argument('--headless')
# chrome_options.binary_location = "/usr/bin/google-chrome"  # Update with the correct path to Chrome binary
chrome_options.add_argument('--disable-dev-shm-usage')

# Create a Service object for Chrome driver
chrome_service = ChromeService(executable_path='/usr/local/bin/chromedriver')

# Create a Chrome driver instance using the Service object and Chrome options
driver = webdriver.Chrome(service=chrome_service, options=chrome_options)

# Navigate to the website
driver.get('https://dsyu.appian.community/suite/')

# Wait for the "I Agree" button to be visible
wait = WebDriverWait(driver, 3)
agree_button = wait.until(EC.visibility_of_element_located((By.XPATH, "//input[@value='I Agree' and @class='btn primary']")))

# Click on the "I Agree" button
agree_button.click()

# Wait for the input box to be visible and enabled
wait = WebDriverWait(driver, 3)
input_box = wait.until(EC.visibility_of_element_located((By.XPATH, "//input[@placeholder='Username' and @id='un']")))

# Send keys to the input box
input_box.send_keys("usernameGoesHere")

# Wait for the input box to be visible and enabled
wait = WebDriverWait(driver, 3)
input_box = wait.until(EC.visibility_of_element_located((By.XPATH, "//input[@placeholder='Password' and @id='pw']")))

# Send keys to the input box
input_box.send_keys("passwordGoesHere")

# Wait for the "Sign In" button to be clickable
wait = WebDriverWait(driver, 3)
signin_button = wait.until(EC.element_to_be_clickable((By.XPATH, "//input[@value='Sign In' and @class='btn primary']")))

# Click on the "Sign In" button
signin_button.click()

# You can continue with your further actions on the website using the Chrome WebDriver.
print("APPEARS TO BE COMPLETED!")
# Close the driver when done
driver.quit()


```


</details>


<details>
<summary>Install Google Chrome into Ubuntu 22.04</summary>

```Python

import subprocess

# Check if Google Chrome is installed
check_cmd = "google-chrome-stable --version"
try:
    output = subprocess.check_output(check_cmd, shell=True, stderr=subprocess.STDOUT)
    print("Google Chrome is already installed.")
    print(output.decode('utf-8'))
except subprocess.CalledProcessError:
    print("Google Chrome is not installed. Proceeding with installation...")

    # Update package lists and upgrade existing packages
    subprocess.run(["sudo", "apt", "update"])
    subprocess.run(["sudo", "apt", "upgrade", "-y"])

    # Install dependencies
    subprocess.run(["sudo", "apt", "install", "-y", "wget", "curl", "unzip"])

    # Download Google Chrome installation package
    subprocess.run(["wget", "https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb"])

    # Install Google Chrome package
    subprocess.run(["sudo", "dpkg", "-i", "google-chrome-stable_current_amd64.deb"])

    # Fix broken dependencies if any
    subprocess.run(["sudo", "apt", "--fix-broken", "install", "-y"])

    # Verify installation
    try:
        output = subprocess.check_output(check_cmd, shell=True, stderr=subprocess.STDOUT)
        print("Google Chrome has been installed successfully.")
        print(output.decode('utf-8'))
    except subprocess.CalledProcessError:
        print("Failed to install Google Chrome.")

```


</details>


<details>
<summary>Create SystemD Service and Timer, Enable it, Start it, Check Status</summary>

```Python
import subprocess

# Create autoLogIntoAppianEveryday2.service file
service_file = """[Unit]
Description=Run autoLogIntoAppianEveryday2 script

[Service]
User=ubuntu
ExecStart=/usr/bin/python3 /home/ubuntu/python/logIntoAppian.py
WorkingDirectory=/home/ubuntu/python
Restart=on-failure

[Install]
WantedBy=multi-user.target"""
with open('autoLogIntoAppianEveryday2.service', 'w') as f:
    f.write(service_file)

# Move autoLogIntoAppianEveryday2.service to /etc/systemd/system/
subprocess.run(["sudo", "mv", "autoLogIntoAppianEveryday2.service", "/etc/systemd/system/"])

# Reload systemd configuration
subprocess.run(["sudo", "systemctl", "daemon-reload"])

# Enable the timer to start automatically at boot
subprocess.run(["sudo", "systemctl", "enable", "autoLogIntoAppianEveryday2.service"])

# Start the service
subprocess.run(["sudo", "systemctl", "start", "autoLogIntoAppianEveryday2.service"])

# Verify service status
subprocess.run(["sudo", "systemctl", "status", "autoLogIntoAppianEveryday2.service"])

# -----------------------------------
# Step 1: Create autoLogIntoAppianEveryday2.timer file
timer_content = """
[Unit]
Description=Run autoLogIntoAppianEveryday2 script every 24 hours

[Timer]
OnBootSec=15min
OnUnitActiveSec=24h
Unit=autoLogIntoAppianEveryday2.service

[Install]
WantedBy=timers.target
"""

with open('autoLogIntoAppianEveryday2.timer', 'w') as f:
    f.write(timer_content)

# Step 2: Move autoLogIntoAppianEveryday2.timer file to /etc/systemd/system/
subprocess.run(['sudo', 'mv', 'autoLogIntoAppianEveryday2.timer', '/etc/systemd/system/'])

# Step 3: Reload SystemD
subprocess.run(['sudo', 'systemctl', 'daemon-reload'])

# Step 4: Enable the timer
subprocess.run(['sudo', 'systemctl', 'enable', 'autoLogIntoAppianEveryday2.timer'])

# Step 5: Start the timer
subprocess.run(['sudo', 'systemctl', 'start', 'autoLogIntoAppianEveryday2.timer'])

# Step 6: Verify timer status
subprocess.run(['sudo', 'systemctl', 'status', 'autoLogIntoAppianEveryday2.timer'])
```


</details>
