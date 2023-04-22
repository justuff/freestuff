# freestuff
<details>
<summary>Automated Login From Ubuntu 22.04 </summary>

```python

# selenium
<details><summary>Automate the Login Into Appian Every Day</summary>

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

# [Timer]
# OnCalendar=*-*-* 00:00:00
# Persistent=true

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
```

</details>
