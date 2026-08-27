# API-Behavior-Monitoring-Script

## Objective 

The goal of this lab was to design and test a Python-based monitoring script capable of evaluating the behavior of a remote API endpoint. The lab focused on building a basic monitoring system that sends HTTP requests to an endpoint and evaluates whether the service is responding normally. In addition to verifying connectivity, the script measures response time, evaluates response size, and detects abnormal conditions such as repeated failures or unexpected status codes. This exercise introduced practical concepts related to service monitoring, API behavior analysis, and basic anomaly detection.

**This project demonstrates how basic monitoring logic can detect abnormal API behavior such as latency spikes, unexpected response codes, and repeated failures. Similar techniques are used in security monitoring systems to identify service degradation, abuse patterns, and potential denial-of-service conditions.**

---

## Breakpoint 1 – Environment Setup and Initial Script

The first step in the lab involved configuring the development environment and creating a basic Python script capable of making HTTP requests. The requests Python library was used to send a GET request to a test endpoint provided by httpbin. This allowed the script to retrieve information about the server response, including the HTTP status code.

The initial version of the script simply verified whether the API endpoint responded successfully. A status code of 200 indicated that the server received, processed, and returned a valid response to the request.

Example logic used in the script:

    import requests

    url = "https://httpbin.org/get"

    response = requests.get(url, timeout=5)

    print("Status Code:", response.status_code)

    if response.status_code == 200:
     print("OK")
    else:
     print("NOT OK")

This portion of the lab established a baseline for determining whether the service was operational and responding correctly.

---

## Breakpoint 2 – Measuring Response Time

The next phase of the lab involved measuring how long the server took to respond to requests. This was done by recording timestamps before and after sending the request using Python’s time module. The difference between the start and end time provided the total response latency.

Example implementation:

    start_time = time.time()

    response = requests.get(url, timeout=5)

    end_time = time.time()

    response_time = end_time - start_time
    
Example output:

    Status Code: 200
    OK


Monitoring response latency is important because unusually slow responses may indicate service degradation, infrastructure issues, or potential denial-of-service conditions.

A threshold was implemented so the script could flag slow responses automatically.

---

## Breakpoint 3 – Detecting Abnormal Response Size

Another monitoring feature added during the lab was response size analysis. The script measured the length of the response body to determine whether the output was unusually large or unexpectedly empty.

Example logic:

    body = response.text
    length = len(body)

Two anomaly checks were added:

- Empty response detection – which could indicate server errors or blocked responses.

- Unusually large responses – which may signal abnormal behavior or misconfigured endpoints.

This step demonstrated how simple heuristics can help detect unexpected application behavior.

---

## Breakpoint 4 – Repeated Failure Detection

The next improvement involved detecting patterns of failure rather than isolated errors. Instead of sending a single request, the script executed multiple requests in a loop and counted how many responses returned an unexpected status code.

Example structure:

    failures = 0

    for i in range(5):

      response = requests.get(url, timeout=5)

      if response.status_code != 200:
        failures += 1

After the loop completed, the script evaluated whether the number of failures exceeded a predefined threshold.

    if failures >= 3:
     print("WARNING: High number of failed requests detected")

This demonstrates the concept of pattern-based detection, which is commonly used in security monitoring systems. Many security tools do not alert on a single event but instead trigger alerts when abnormal patterns emerge over time.

---

## Breakpoint 5 – Testing Abnormal Conditions

The script was tested against multiple simulated scenarios using endpoints provided by httpbin:

403 Forbidden endpoint:

  - https://httpbin.org/status/403

This endpoint consistently returns a 403 response, allowing the script to simulate repeated access failures. The script successfully detected multiple failed responses and triggered the failure-rate warning.

Delayed response endpoint:

  - https://httpbin.org/delay/3

This endpoint introduces a three-second delay before responding. When tested, the script correctly identified the slow response and triggered the latency warning.

These tests demonstrated that the monitoring logic successfully identified abnormal service behavior.

---

## Full Script 

    import requests # Importing the requests library to make HTTP requests
    import time # Importing the time library to add delays between requests

    url = "https://httpbin.org/status/403" # The URL to which we will send the GET request

    failures = 0 # Initializing a counter to keep track of failed requests


     for i in range(5): # Sending the GET request 5 times
         start_time = time.time() # Recording the start time before sending the request

         response = requests.get(url, timeout=5) # Sending the GET request again to measure the response time   

         end_time = time.time() 

         response_time = end_time - start_time # Calculating the response time by subtracting the start time from the end time

         print(f"Request {i+1}:") # Printing the request number
         print("Status Code:", response.status_code) # Printing the status code of the response

         print("Response time:", response_time)

       if response.status_code != 200: # Checking if the status code of the response is not 200
        failures += 1 # If it is not, increment the failure count
  

    if response_time > 2: # Checking if the response time is greater than 2 seconds
       print("WARNING: Slow response detected") # If it is, print a warning message 
    else: 
       print("Response time normal") # If it is not, print a message indicating that the response time is acceptable  

    body = response.text # Getting the body of the response as text
    length = len(body) # Calculating the length of the response body

    print("Response length:", length) # Printing the length of the response

    if length > 5000:
       print("WARNING: Unusually Large response") # If the length of the response is greater than 5000 characters, print a warning message
    elif length == 0:
       print("WARNING: Empty response") # If the length of the response is 0, print a warning messag
    else: 
       print("Response size normal") # If the length of the response is within normal limits, print a message indicating that it is acceptable

    if response.status_code != 200: # Checking if the status code of the response is not 200
       print("WARNING: Unexpected status code") # If it is not, print a warning message

    if failures > 3 : # Checking if there were any failed requests
       print("WARNING: High number of failed requests detected") # If there were, print a warning message  
    else:
       print("Failure rate normal") # If there were not, print a message indicating that the number of failed requests is acceptable

## Output:

    Response time: 2.58
    Status Code: 200
    WARNING: Slow response detected
    Response length: 307
    Response size normal

and

    Status Code: 403
    WARNING: Unexpected status code
    WARNING: High number of failed requests detected

---

# Key Findings & Security Implications (Reflection)

 One challenge encountered during this lab was troubleshooting environment configuration issues when working between Windows and the Linux-based WSL environment. Permission errors and interpreter mismatches initially prevented the script from running correctly. Resolving       these issues helped improve my understanding of how development environments and file permissions affect script execution.

 This lab also demonstrated how simple Python logic can be used to monitor API behavior. By measuring response time, evaluating response size, and detecting repeated failures, the script was able to identify abnormal conditions such as delayed responses and forbidden           requests. These concepts relate directly to how monitoring systems detect service disruptions or suspicious activity.

 Overall, this lab reinforced how scripting can be used to automate monitoring tasks and detect abnormal behavior in networked services, which is an important capability in cybersecurity and system monitoring.
