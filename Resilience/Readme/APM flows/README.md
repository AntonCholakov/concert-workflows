
# Prerequisites for All Workflow Execution

## Create Authentication Configurations

You can use one of the following authentication methods:

#### IBM Hub - Self (Recommended)
Go to the **Workflows > Authentications** tab, select **IBM Hub - Self** as the service, provide a name for your authentication, and click **Create**.


#### IBM Concert API Key
Go to the **Workflows > Authentications** tab, select **IBM Concert API Key** as the service, provide the following details, and click **Create**:

- `Protocol`: Select the appropriate protocol (e.g., `https`)
- `Host`: Your Concert domain with port (e.g., `example.com:8000`)
- `Instance ID`: Your Concert instance ID
- `API Key Type`: Select `C_API_Key`
- `API Key`: Your Concert API key secret
---

# Dynatrace Resilience (service-level) Workflow Integration with Concert Workflows

## Overview
This document provides instructions for integrating **Dynatrace service-level workflows** within the **Resilience** folder of Concert Workflows. Follow these steps to configure your Dynatrace environment and ensure successful workflow execution.

## Prerequisites for Workflow Execution
Before importing and running the workflow in Concert, ensure the following prerequisites are completed:

### 1. Upload Application to Concert
- Upload the application that is being monitored by **Dynatrace** to the **Concert** platform.

### 2. Create Authentication Configurations
### 3.1 Concert Authentication

- Go to **Workflows > Authentications** and create an authentication for Concert.  
- You can use either:  
**IBM Hub - Self (Recommended):**  
Go to the **Workflows > Authentications** tab, select **IBM Hub - Self** as the service, provide a name, and click **Create**.  

**IBM Concert API Key:**  
Go to the **Workflows > Authentications** tab, select **IBM Concert API Key** as the service, provide the required details, and click **Create**.
- You must configure **Dynatrace authentication** to enable secure communication between Concert and Dynatrace.  
- This ensures the workflow can access metrics and data during execution.

**Dynatrace Authentication requirements:**
- **Domain** – The Url of your Dynatrace Account (e.g., `https://zgb79854.apps.dynatrace.com/`)
- **Environment ID** – The Environment ID of your Dynatrace Account (e.g., `yrg32424`)
- **Access Token** – retrieved from your Datadog account.  

---

## Input to the workflow

![image](https://github.ibm.com/roja/concert-workflows/assets/481485/8de6145b-5085-4a6f-890a-e25bfc65df81)

### Description:
- applicationVersion – The SBOM version of your application that is loaded in Concert.
- origin_name – Set to Dynatrace, as this is the source from which the metrics are retrieved.
- origin – Set to APM, as the collected data pertains to APM (Application Performance Monitoring) metrics.
- origin_url – Your Dynatrace account URL.
- application_name – The name of your application, either as registered in Concert or as monitored via Dynatrace.
- profile_name – The profile name associated with the posture created in Step 3 of the Prerequisites.
- defaultWindowSize – The time window or duration for which metrics should be fetched.
- service_id - Id of the service which we wanted a metric for.
- Synthetic_test_id - Id of the synthetic test for which we need to metric 
- concertAPIKey – The path where the Concert API key needs to be provided.
- dynatrace_auth – The path where Dynatrace authentication details should be specified.

## Running the Workflow

### 1. Execute the Workflow
- Run the workflow in Concert.  
- After execution, an **assessment_id** will be generated.

![image](https://github.ibm.com/roja/concert-workflows/assets/481485/a462fcb8-b804-4c04-a00d-b3f349287a4e)

---

### 2. View Metrics in Concert
- Go to **Concert → Dimensions → Resilience**.  
- Select your posture → select the generated **assessment**.  
- You will see **service-level metrics** retrieved from Dynatrace.

![image](https://github.ibm.com/roja/concert-workflows/assets/481485/bd62b653-f9b0-413a-9fd8-748c0d363593)

----

## 📝 Note on Metric Filtering Behavior

When the Dynatrace API response for a specific metric contains **`null`** or **`0`** values for the given timestamp, the workflow **automatically filters out those metrics** during processing.

This means:
- Only metrics with **valid (non-null and non-zero)** values are included in the final output.  
- Metrics that return only null or zero values within the selected time window will **not appear in the assessment results**.

This behavior is **intentional** to ensure that the posture and assessment contain only meaningful data for analysis and reporting.

----

# Dynatrace Application Resilience (application-level) Workflow Integration with Concert Workflows

## Overview
The **Dynatrace Application-Level Workflow** is designed to collect **Non-Functional Requirements (NFRs)** directly from Dynatrace and load them into IBM Concert for posture and assessment creation.

## Prerequisites for Workflow Execution
Before importing and running the workflow in Concert, ensure the following prerequisites are completed:

### 1. Upload Application to Concert
- Upload the application that is being monitored by **Dynatrace** to the **Concert** platform.

### 2. Create Authentication Configurations
### 3.1 Concert Authentication

- Go to **Workflows > Authentications** and create an authentication for Concert.  
- You can use either:  
**IBM Hub - Self (Recommended):**  
Go to the **Workflows > Authentications** tab, select **IBM Hub - Self** as the service, provide a name, and click **Create**.  

**IBM Concert API Key:**  
Go to the **Workflows > Authentications** tab, select **IBM Concert API Key** as the service, provide the required details, and click **Create**.
- You must configure **Dynatrace authentication** to enable secure communication between Concert and Dynatrace.  
- This ensures the workflow can access metrics and data during execution.

**Dynatrace Authentication requirements:**
- **Domain** – The Url of your Dynatrace Account (e.g., `https://zgb79854.apps.dynatrace.com/`)
- **Environment ID** – The Environment ID of your Dynatrace Account (e.g., `yrg32424`)
- **Access Token** – retrieved from your Datadog account. 

---

## Input to the workflow

![image](https://github.ibm.com/roja/concert-workflows/assets/481485/338bd612-bd30-4f23-aa85-b9792288e44f)

### Description:
- result – A placeholder variable to store the final computed output of the workflow.  
- applicationVersion – The SBOM version of your application that is loaded in Concert (e.g., 5.1.0).  
- origin_name – Set to "Dynatrace", as this is the source from which the metrics are retrieved.  
- origin – Set to "APM", as the collected data pertains to Application Performance Monitoring metrics.  
- origin_url – Your Dynatrace account URL (e.g., `https://<your-env>.live.dynatrace.com`).  
- application_name – The name of your application, either as registered in Concert or as monitored via Dynatrace (e.g., quote_of_the_day).  
- profile_name – The profile name associated with the posture created for this application (e.g., nfr_critical_apps_profile).  
- defaultWindowSize – The time window or duration for which metrics should be fetched (e.g., 30000 milliseconds).  
- startTimestamp – Start time in epoch milliseconds for the metrics retrieval window.  
- endTimestamp – End time in epoch milliseconds for the metrics retrieval window.  
- endTime – Alternative end time in epoch milliseconds, used in some workflow branches.  
- concertAPIKey – The path where the Concert API key needs to be provided (e.g., ibmconcert/ConcertApiKey).  
- dynatrace_auth – The path where Dynatrace authentication details should be specified (API token).  
- managementZone – The name of the Dynatrace management zone in which the application resides (e.g., QOTD-MZ).  
- operation – Defines the aggregation method for each metric.

```json
{
  "pct_slo_error_rate_errorbudget": "avg",
  "num_throughput_transactions": "max",
  "pct_availability_by_error_rate": "avg",
  "pct_availability_by_synthetic_tests": "max"
}
```

## Running the Workflow

### 1. Execute the Workflow
- Run the workflow in Concert.  
- After execution, an **assessment_id** will be generated.  

![image](https://github.ibm.com/roja/concert-workflows/assets/481485/dff0c8d1-0d92-419e-8d8e-a7a69b2010ce)

---

### 2. View Metrics in Concert
- Go to **Concert → Dimensions → Resilience**.  
- Select your posture → select the generated **assessment**.  
- You will see **service-level metrics** retrieved from Datadog.

![image](https://github.ibm.com/roja/concert-workflows/assets/481485/6c1132b8-3f47-49f9-a0ea-74a529b02297)

----

## 📝 Note on Metric Filtering Behavior

When the Dynatrace API response for a specific metric contains **`null`** or **`0`** values for the given timestamp, the workflow **automatically filters out those metrics** during processing.

This means:
- Only metrics with **valid (non-null and non-zero)** values are included in the final output.  
- Metrics that return only null or zero values within the selected time window will **not appear in the assessment results**.

This behavior is **intentional** to ensure that the posture and assessment contain only meaningful data for analysis and reporting.

----

# Datadog Resilience (service-level) Workflow Integration with Concert Workflows

## Overview
This document provides instructions for integrating **Datadog service-level workflows** within the **Resilience** folder of Concert Workflows. Follow these steps to configure your Datadog environment and ensure successful workflow execution.

## Prerequisites for Workflow Execution
Before importing and running the workflow in Concert, ensure the following prerequisites are completed:

### 1. Upload Application to Concert
- Upload the application that is being monitored by **Datadog** to the **Concert** platform.

### 2. Create Authentication Configurations
### 3.1 Concert Authentication

- Go to **Workflows > Authentications** and create an authentication for Concert.  
- You can use either:  
**IBM Hub - Self (Recommended):**  
Go to the **Workflows > Authentications** tab, select **IBM Hub - Self** as the service, provide a name, and click **Create**.  

**IBM Concert API Key:**  
Go to the **Workflows > Authentications** tab, select **IBM Concert API Key** as the service, provide the required details, and click **Create**.
- You must configure **Datadog authentication** to enable secure communication between Concert and Datadog.  
- This ensures the workflow can access metrics and data during execution.

**Datadog Authentication requirements:**
- **Host** – The Datadog API endpoint for your region (e.g., api.us5.datadoghq.com)
- **API Key** – retrieved from your Datadog account.  
- **Application Key** – generated from your Datadog account profile.  

---

## Input to the workflow

![image](https://github.ibm.com/roja/concert-workflows/assets/481485/1c288a17-da25-42c9-a809-3def6264abcf)

### Description:
- origin – Set to APM, as the collected data pertains to APM (Application Performance Monitoring) metrics.
- origin_name – Set to Datadog, as this is the source from which the metrics are retrieved.
- origin_url – Your Datadog account URL. The url will be `https://<region>.datadoghq.com` for example if your datadog account is based in us5 region the origin_url will be `https://us5.datadoghq.com`.
- applicationVersion – The SBOM version of your application that is loaded in Concert.
- application_name – The name of your application, either as registered in Concert or as monitored via Datadog.
- profile_name – The profile name associated with the posture created in Step 3 of the Prerequisites.
- defaultWindowSize – The time window or duration for which metrics should be fetched.
- service_name - The service which you want to get metrics for.
- env_name - Env name is which where the service is running like prod, test etc.
- syn_test_id - Id of synthetic test for which you want to have metric for. The test ID will be generated once the synthetic test is created. The ID is usually a string separated by '-' like `tcw-hji-piy`.
- concertAPIKey – The path where the Concert API key needs to be provided.
- concertConfigAuth – The path for configuring authentication related to Concert.
- datadog_auth – The path where Datadog authentication details should be specified.

## Running the Workflow

### 1. Execute the Workflow
- Run the workflow in Concert.  
- After execution, an **assessment_id** will be generated.  

![image](https://github.ibm.com/roja/concert-workflows/assets/481485/320b4085-e66f-476b-adfd-d0f23f7d42a9)

---

### 2. View Metrics in Concert
- Go to **Concert → Dimensions → Resilience**.  
- Select your posture → select the generated **assessment**.  
- You will see **service-level metrics** retrieved from Datadog.

![image](https://github.ibm.com/roja/concert-workflows/assets/481485/3901fcfe-b89b-47b8-96fa-02157a28b185)

---

# Datadog Application Resilience (application-level) Workflow Integration with Concert Workflows

## Overview
The **Datadog Application-Level Workflow** is designed to collect **Non-Functional Requirements (NFRs)** directly from Datadog and load them into IBM Concert for posture and assessment creation.

## Prerequisites for Workflow Execution
Before importing and running the workflow in Concert, ensure the following prerequisites are completed:

### 1. Upload Application to Concert
- Upload the application that is being monitored by **Datadog** to the **Concert** platform.

### 2. Create Authentication Configurations
### 3.1 Concert Authentication

- Go to **Workflows > Authentications** and create an authentication for Concert.  
- You can use either:  
**IBM Hub - Self (Recommended):**  
Go to the **Workflows > Authentications** tab, select **IBM Hub - Self** as the service, provide a name, and click **Create**.  

**IBM Concert API Key:**  
Go to the **Workflows > Authentications** tab, select **IBM Concert API Key** as the service, provide the required details, and click **Create**.
- You must configure **Datadog authentication** to enable secure communication between Concert and Datadog.  
- This ensures the workflow can access metrics and data during execution.

**Datadog Authentication requirements:**
- **Host** – The Datadog API endpoint for your region (e.g., api.us5.datadoghq.com)
- **API Key** – retrieved from your Datadog account.  
- **Application Key** – generated from your Datadog account profile.  

---

## Input to the workflow

![image](https://github.ibm.com/roja/concert-workflows/assets/481485/7990ff2d-c841-4b45-aa48-a919354a2596)

### Description:
- origin – Set to "APM", as the collected data pertains to Application Performance Monitoring metrics.
- origin_name – Set to "Datadog", as this is the source from which the metrics are retrieved.
- origin_url – Your Datadog account URL (e.g., `https://us5.datadoghq.com`).
- applicationVersion – The SBOM version of your application that is loaded in Concert (e.g., 5.1.0).
-	applicationName – The name of your application, either as registered in Concert or as monitored via Datadog (e.g., quote_of_the_day).
-	profileName – The profile name associated with the posture created for this application (e.g., nfr_critical_apps_profile).
-	envName – The environment in which the service is running, such as dev or prod.
-  teamName – The team or business unit responsible for the application/service. This helps categorize metrics and postures.  
-	defaultWindowSize – The time window or duration for which metrics should be fetched (e.g., 30000 milliseconds).
-	endTime – Alternative end time in epoch milliseconds, used in some workflow branches.
-	concertAPIKey – The path where the Concert API key needs to be provided (e.g., ibmconcert/concert_auth).
-	concertConfigAuth – The path for configuring authentication related to Concert (e.g., ibmconcert/configAuth).
-	datadog_auth – The path where Datadog authentication details should be specified (API key + Application key).
-	operation – Defines the aggregation method for each metric.

```json
  {
    "num_throughput_transactions": "max",
    "pct_availability_by_error_rate": "max",
    "ms_slo_latency": "avg",
    "pct_availability_by_synthetic_tests": "max"
  }
  ```

## Running the Workflow

### 1. Execute the Workflow
- Run the workflow in Concert.  
- After execution, an **assessment_id** will be generated.  

![image](https://github.ibm.com/roja/concert-workflows/assets/481485/76dcdd2b-0393-4280-b7eb-8b3130cd9256)

---

### 2. View Metrics in Concert
- Go to **Concert → Dimensions → Resilience**.  
- Select your posture → select the generated **assessment**.  
- You will see **service-level metrics** retrieved from Datadog.

![image](https://github.ibm.com/roja/concert-workflows/assets/481485/0e8325fd-1f5a-4dae-8246-58b9bd45b296)

---

# Instana Workflow Integration with Concert Workflows

## Overview
This workflow is designed to run Instana resilience tests within the IBM Concert platform. It involves authentication with Instana, API key authentication, and selecting configuration profiles.

## Prerequisites for Workflow Execution

Once you have completed the below prerequisites, you should be ready to import and execute your instana workflow within Concert Workflows. If you encounter any issues, ensure all settings are properly configured and the necessary credentials are correct.

### 1. Pull the application using ingestion job
                1.Navigate to administration --> integrations.
                2.Click on connections -→ Create connection
                3.Search for instana --> select IBM instana Observability.
                4.name the connection
                5.origin_url: Provide instana instance url like https://<instana_instance_domain>.instana.io
                6.give the apikey as <Api key>
                7.click validate connection
                8.click on create to create connection
            Create the ingestion job
                1.Navigate to administration --> integrations.
                2.Click on ingestion jobs -→ Create ingestion job
                3.name the ingestion job
                4.select the connection type as instana.
                5.select the connection.
                6.select the target ennvironment
                7.click on create to create injection job
                8.run the ingestion job

### 2. Enable Resilience Settings
- Enable the **Resilience** settings in the following areas:
  - **Main Settings** of Concert
  - **Application Settings** within Concert


### 3. Create Authentication Configurations
- You will need to create the appropriate authentication configurations to enable secure communication between Concert and Instana. This ensures that the workflow can access required resources and data during execution. 

   **Instana Authentication:**
   - Go to the Workflows tab in IBM Concert.
   - Navigate to Authentications.
   - Click Create Authentication.
   - Select Service as IBM instana.
   - Provide the following  details:
       ```bash
        Subdomain :<instana_instance_subdomain> # Only the subdomain, not the full URL
        API Key : <api_key> 
        ```
   - Save the authentication.

For all other authentication types, please refer to the  [Create Authentication Configurations](#create-authentication-configurations) section.

### 4. Provide inputs to run the workflow
To determine which inputs need to be passed to your workflow, refer to the images below. Additionally, explanation is provided for some of the inputs to help you understand what each one represents.

<img width="1042" alt="Screenshot 2025-04-08 at 3 53 00 PM" src="https://github.ibm.com/roja/concert-workflows/assets/481485/041191a4-29f3-4a87-96a7-4de196467414">

#### Description:
- application_name – The name of your application, either as registered in Concert or as monitored via Instance.
- profile_name – The profile name associated with the posture created in Step 3 of the Prerequisites.
- defaultWindowSize – The time window or duration for which metrics should be fetched.
- concertAPIKey – The path where the Concert API key needs to be provided.
- concertConfigAuth – The path for configuring authentication related to Concert.
- instanaAuth – The path where Instana authentication details should be specified.
- originUrl - Provide your instana instance domain
- webApplicationName - The web application name of your application

Note: Synthetic Test Setup is not mandatory for workflow execution.

## Run workflow

### 1. You will see the below output after running 
You can see the generated assessment_id 

<img width="1728" alt="Screenshot 2025-04-08 at 3 59 48 PM" src="https://github.ibm.com/roja/concert-workflows/assets/481485/c70bc6e4-ae32-4559-8d44-5813b58ec267">

### 2. Go to concert >> Dimensions  >> Resilience >> click on your posture >> click on the assesment
 You can see the metrices which is coming from instana 

<img width="1728" alt="Screenshot 2025-04-08 at 4 01 33 PM" src="https://github.ibm.com/roja/concert-workflows/assets/481485/bb018690-912c-45df-8de5-0bfa330d3b5e">

---

# Image Registry Workflow
## Overview
This document provides instructions for Image Registry Workflow within the **Resilience** folder of Concert Workflows.
The image registry workflow will capture the metrics for the images that are not deployed in any environment, but the images are only present in the registry. The workflow will pull those images into an VM, and then inspect the images to compute the metrics related to image layers and size.

## Prerequisites for Workflow Execution

Once you have completed the below prerequisites, you should be ready to import and execute your image registry workflow within Concert Workflows.

### 1. Upload Application to Concert
- The application for which the images are provided as input to the workflow should be present in Concert.

### 2. Inputs to the Workflow
`Concert API Key Auth` - The API key authentication of the Concert Instance.

`VM Auth` - SSH Auth of the VM in which the images will be pulled.

`Concert Config Auth` - Config Auth of the Concert Instance.

  ```json
     {
       "concert_host": "test.fyre.ibm.com",         // Concert VM host URL
       "concert_port": "12443",                     // Concert VM port
       "concert_instance_id": "0000-0000-0000-0000" // Concert VM instance ID
     }
  ```


`Registry Config Auth` - Config Auth of the Registry from which the images are to be pulled.

  ```json
     {
       "registry_username": "sample_username", // Registry Username
       "registry_password": "sample_password", // Registry Password
       "registry_url": "sample_registry_url" // Registry URL
     }
  ```

`Application Name` - Name of the application of which the images are pulled.

`Application Version` - Version of the application.

`Public Registry boolean` - This boolean should have value as `true` if the registry is `public` and it should have value as `false` if the registry is `private`

`Image List` - Array of Images to be pulled.

`docker_command` - Docker command for the VM provided in VM Auth, by default it is set to `docker`

### After configuring all the necessary input variables and authentication details, save the workflow.
#### Click Run to execute the workflow.
After the execution of the Workflow, The assesment will be created in the `Resilience` tab.

---

# Package_Vulnerability_Exposure_Assessment- Workflow
## Overview
This document provides instructions for Package_Vulnerability_Exposure_Assessment Workflow within the **Resilience** folder of Concert Workflows.

**Functionality**:

Pull aggregated metrics for Packages, vulnerabilities and Exposures for the given application and
Load the Assessment data to IBM Concert for all the profiles associated with that application.

Metrics computed -

**Packages** - Unpermitted packages,Backlevel packages,Outdated packages

**Open Exposures** - num_priority1_exposures ,num_priority2_exposures,num_priority3_exposures,num_deprioritized_exposures

**Open Vulnerabilities** - num_priority1_vulnerabilities ,num_priority2_vulnerabilities, num_priority3_vulnerabilities,num_deprioritized_vulnerabilities


## Prerequisites for Workflow Execution

Once you have completed the below prerequisites, you should be ready to import and execute your Package_Vulnerability_Exposure_Assessment workflow within Concert Workflows.

### 1. Upload Application and respective packages,images scan and exposures scan files to Concert
- The application for which the package, vulnerability, and exposure metrics need to be retrieved must be available in Concert..
- Upload the respective packages,images scan and exposures scan files to Concert.

### 2. Create a Posture for the Application
Navigate to the Resilience tab within Concert and create a Posture for your application. Use your specific profile to configure this posture.

### 3. Inputs to the Workflow
`Concert API Key Auth` - The API key authentication of the Concert Instance.

service name for API Key Auth-  API Key


`Concert Config Auth` - Config Auth of the Concert Instance.

  ```json
     {
       "concert_host": "test.fyre.ibm.com",         // Concert VM host URL
       "concert_port": "12443",                     // Concert VM port
       "concert_instance_id": "0000-0000-0000-0000" // Concert VM instance ID
     }
  ```


`Application Name` - Name of the application of which the mertics needs to be retrieved.

`Application Version` - Version of the application.


### After configuring all the necessary input variables and authentication details, save the workflow.
#### Click Run to execute the workflow.
After the execution of the Workflow, The assesment will be created in the `Resilience` tab.

---
