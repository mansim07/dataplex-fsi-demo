# dataplex-fsi-demo

This repository is a one-click deployment for setting up Dataplex FSI demo. 

1. Create a GCP Project 
2. Grant the below IAM permissions to your account<br>
    - Organization Administrator
    - Owner
    - Service Account Token Creator
    - Service Account User

3. [![Open in Cloud Shell](http://gstatic.com/cloudssh/images/open-btn.svg)](https://console.cloud.google.com/cloudshell/editor)

4. Install the below python libraries 

    ```bash
    pip3 install google-cloud-storage
    pip3 install numpy 
    pip3 install faker_credit_score
    ```
5. Setup Environment variable 
- In cloud shell, declare the following variables after substituting with yours. 
- Use fully qualified email address e.g. joe.user@gmail.com as the USERNAME

    ```bash
    echo "export USERNAME=your-email" >> ~/.profile
    ```
- Setup your project ID. Execute the below command    
    ```
    echo "export PROJECT_ID=$(gcloud config get-value project)" >> ~/.profile
    ```

- Verify setup
    ```
    cat ~/.profile
    ```

    ![profile_validate](/setup/resources/code_artifacts/imgs/profile-validate.png)

6. To get the currently logged in email address, run: 'gcloud auth list as' below:

    ```bash 
    gcloud auth list
    Credentialed Accounts

    ACTIVE: *
    ACCOUNT: joe.user@jgmail.com or admin@(for Argolis)
    ```

7. Clone this repository in Cloud Shell
   ```bash 
   git clone https://github.com/mansim07/dataplex-fsi-demo.git
   ```

8. Trigger the terraform script to setup the infrastructure 

    ```bash 
    cd ~/dataplex-fsi-demo/setup/
    source ~/.profile
    bash deploy-helper.sh ${PROJECT_ID} ${USERNAME}
    ```
     The script will take about 30-40 minutes to finish.

9. Request for a CPU quota increase in your project region 

10. Go to Airflow and Trigger the master dags

11. Execute the below commands to populate the overview 
```bash 
export PROJECT_ID=$(gcloud config get-value project)
entry_name=`curl -X GET -H "x-goog-user-project: ${PROJECT_ID}" -H  "Authorization: Bearer $(gcloud auth print-access-token)" -H "Content-Type: application.json" "https://datacatalog.googleapis.com/v1/entries:lookup?linkedResource=//bigquery.googleapis.com/projects/${PROJECT_ID}/datasets/customer_data_product/tables/customer_data&fields=name" | jq -r '.name'`



curl -X POST -H "x-goog-user-project: d${PROJECT_ID}" -H  "Authorization: Bearer $(gcloud auth print-access-token)" -H "Content-Type: application.json" https://datacatalog.googleapis.com/v1/${entry_name}:modifyEntryOverview -d "{\"entryOverview\":{\"overview\":\"<header><h1>Customer Demograhics Data Product</h1></header><br>This customer data table contains the data for customer demographics of all Bank of Mars retail banking customers. It contains PII information that can be accessed on need-to-know basis. <br> Customer data is the information that customers give us while interacting with our business through websites, applications, surveys, social media, marketing initiatives, and other online and offline channels. A good business plan depends on customer data. Data-driven businesses recognize the significance of this and take steps to guarantee that they gather the client data points required to enhance client experience and fine-tune business strategy over time.\"}}"

```