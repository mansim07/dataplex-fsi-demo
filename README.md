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
- For Argolis, use fully qualified email address e.g. joe.user@gmail.com as the USERNAME

    ```bash
    echo "export USERNAME=your-email" >> ~/.profile
    echo "export PROJECT_ID=$(gcloud config get-value project)" >> ~/.profile
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
    cd ~/dataplex-labs/setup/
    source ~/.profile
    bash deploy-helper.sh ${PROJECT_ID} ${USERNAME}
    ```
     The script will take about 30-40 minutes to finish.