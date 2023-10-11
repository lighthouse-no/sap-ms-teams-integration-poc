# Initial Setup

1. Install Visual Studio Code
1. Ensure Sideloading for Teams Apps is enabled

   From your Teams Admin Centre, select "Teams apps" --> "Setup policies"

   ![Teams Setup Policies](../img/teams_setup_policies.png)

   Select the "Global (Org-wide default)" policy and switch on "Upload custom apps"

   ![Upload custom apps](../img/upload_custom_apps.png)

   The non-intuitive part here is that Microsoft uses inconsistent terminology.

   The term "sideloading" is used in the Teams Toolkit software, but is not used in the documentation.
   Instead, the documentation almost always refers to "Uploading custom apps"

   Without this setting switched on, you will not be able to test your Teams app without first making it available to your entire organisation.

2. Check that sideloading has been correctly enbled.

   In MS Teams, click on <img src="../img/icon_apps.png" /> at the bottom of the toolbar on the left, then select <img src="../img/icon_manage_your_apps.png" /> at the bottom of the screen and then <img src="../img/icon_upload_an_app.png" />

   If sideloading has been enable, you will see this option ![Upload customised app](../img/upload_customised_app.png)
3. In VSCode, install the Teams Toolkit plugin in and connect to your Teams installation using your admin account.
   When the plugin connects to your Teams account, it will automatically check whether sideloading is enabled.
   If it is not, this is considered an error as although testing is possible, it is not nearly so convenient.
