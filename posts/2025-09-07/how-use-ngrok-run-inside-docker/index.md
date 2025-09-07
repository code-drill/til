<!--
.. title: how use ngrok run inside docker
.. slug: how-use-ngrok-run-inside-docker
.. date: 2025-09-07 16:15:32 UTC+02:00
.. tags: ngrok,docker,localhost,development
.. category: development
.. link: 
.. description: 
.. type: text
-->

If you wish use ngrok, but dont want to install another system app, you can run it using docker.

### Steps ###

1. go to: [https://dashboard.ngrok.com/get-started/your-authtoken](https://dashboard.ngrok.com/get-started/your-authtoken)

2. copy the auth token

3. run the below command line with the appropriate `<TOKEN>` (from the above step) and `<PORT>` of the running app:
   ```shell
   docker run -it -e NGROK_AUTHTOKEN=<TOKEN> --add-host=host.docker.internal:host-gateway ngrok/ngrok http host.docker.internal:<PORT>
   ```
   _please note that you should add `host.docker.internal` and use it instead of `127.0.0.1` or `localhost` for your host_

4. copy the provided url and share your app