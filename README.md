# Packer Script Base Image
Using packer to create the base image and bash scripts to provision the configs on gcp compute engine.

## To Get Started
- use the `git clone` command to clone the project
    'git clone https://github.com/JanetJanx/packer-script-base-image.git'

- login to your google console project, create a service account with the required minimum roles, create the      key for the service account and download it as `json`.

- Create the json service account file (use a name of your choice) within the clonned project and copy paste the downloaded key.

- Edit the `packerfile.json` file to remove the `account.json` value for the `service_account_json`              variable under the `variables` section and replace it with the name of the created service account file.

- Run `packer build packerfile.json` command to build the image. After completion, you should see something      like this below.
    ```
    ==> googlecompute: Deleting instance...
        googlecompute: Instance has been deleted!
    ==> googlecompute: Creating image...
    ==> googlecompute: Deleting disk...
        googlecompute: Disk has been deleted!
    Build 'googlecompute' finished.

    ==> Builds finished. The artifacts of successful builds are:
    --> googlecompute: A disk image was created: ubuntujanxscript-18
    ```

## Test the base image
    Create a vm instance on gcp compute engine and use the created image which you will find under the custom images. ssh the vm and check whether the service is running using `sudo service css_clock status` command to see something like this below

    ```
    username@testing:~$ sudo service css_clock status
    ● css_clock.service - Start CSS clock
    Loaded: loaded (/etc/systemd/system/css_clock.service; enabled; vendor preset: enabled)
    Active: active (running) since Sat 2019-01-26 12:39:39 UTC; 17s ago
    Main PID: 1771 (node)
        Tasks: 30 (limit: 4401)
    CGroup: /system.slice/css_clock.service
            ├─1771 node /usr/share/yarn/bin/yarn.js start
            ├─1807 /bin/sh -c PORT=80 react-scripts start
            ├─1808 /usr/bin/node /home/packer/css_clock/node_modules/.bin/react-scripts start
            └─1815 /usr/bin/node /home/packer/css_clock/node_modules/react-scripts/scripts/start.js
    Jan 26 12:39:39 testing systemd[1]: Started Start CSS clock.
    Jan 26 12:39:39 testing bash[1771]: yarn run v1.13.0
    Jan 26 12:39:39 testing bash[1771]: $ PORT=80 react-scripts start
    Jan 26 12:39:41 testing bash[1771]: Starting the development server...
    Jan 26 12:39:44 testing bash[1771]: Compiled successfully!
    Jan 26 12:39:44 testing bash[1771]: You can now view clock in the browser.
    Jan 26 12:39:44 testing bash[1771]:   Local:            http://localhost:80/
    Jan 26 12:39:44 testing bash[1771]:   On Your Network:  http://10.142.0.23:80/
    Jan 26 12:39:44 testing bash[1771]: Note that the development build is not optimized.
    Jan 26 12:39:44 testing bash[1771]: To create a production build, use yarn build.
    ```