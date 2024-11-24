# Configure SonarQube

Switch to the sonarqube directory, modify `<your admin password>` in the configuration item in the charts/sonarqube/values.yaml file, then install it.

https://raw.githubusercontent.com/rh-demos/agnosticg/refs/heads/main/charts/sonarqube/values.yaml

```
cd charts/sonarqube
./install.sh
```

Get the sonarqube URL and access it, accepting the risk of the plugin.

![image-20241030213005833](assets/4-2-configure-sonarqube/image-20241030213005833.png)

Check and make sure that the Community Branch Plugin and Reporting Plugin are installed successfully.

![image-20241030213042359](assets/4-2-configure-sonarqube/image-20241030213042359.png)

Configure the URL of sonarqube from the `General Configuration` options and save.

![image-20241030213210847](assets/4-2-configure-sonarqube/image-20241030213210847.png)

Confiure the sonarqube Access Token from the gitlab page and save it for later.

![image-20241030213309328](assets/4-2-configure-sonarqube/image-20241030213309328.png)

Add gitlab configuration in the `DevOps Platform Integrations` configuration on the sonarqube page.

![image-20241030213354471](assets/4-2-configure-sonarqube/image-20241030213354471.png)

Enter the name of the configuration: `gitlab`, `the URL` of gitlab and the `token` created previously.

![image-20241030213450546](assets/4-2-configure-sonarqube/image-20241030213450546.png)

The configuration verification succeeded.

![image-20241030213559551](assets/4-2-configure-sonarqube/image-20241030213559551.png)

