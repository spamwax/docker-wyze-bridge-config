# Setting up a Wyze camera for mainsail (klipper)

I needed to use a Wyze camera on my 3D printer and since my macMini is always on (and is quiet with no fans) I decided to run [docker-wyze-bridge](https://github.com/mrlt8/docker-wyze-bridge) on it.

However, docker on macOS requires `Docker Desktop` to be installed and I didn't want to have another software running in the background I decided to use `podman` since it seems to be lighter to install and maintain.

### Step 1: Install podman

First, make sure you have podman installed on your system. You can install it using Homebrew:

`brew install podman && brew install podman-compose`.

### Step 2: Clone this repository

Open your terminal and navigate to the directory where you want to clone the repository. Then, run:

```bash
git clone https://github.com/spamwax/docker-wyze-bridge-config
```

### Step 3:

Navigate to the cloned repository and add a `.env` file with your credentials:

```
WYZE_EMAIL=__your_wyze_email__
WYZE_PASSWORD=__your_wyze_password__
API_ID=__your_api_id__
API_KEY=__your_api_key__
WB_USERNAME=__wb_username__
WB_PASSWORD=__wb_password__
WB_IP=
KLIPPER_IP=
CAMERA_NAME=
```

Replace the placeholders with your actual Wyze [Smart Home credentials](https://developer-api-console.wyze.com/#/apikey/view) and your desired credentials for accessing the web UI.
Additionally, set `CAMERA_NAME` to the name of your camera from your Wyze account.
Also in the same `.env` file, set `WB_IP` to the IP of your host computer.
You can also set `KLIPPER_IP` to the IP of raspberrypi running klipper.

### Step 4: Build and run the docker-wyze-bridge

In the terminal, navigate to the cloned repository and run:

```bash
podman-compose up
```
Note that for the Web UI to work on macOS, you need to use the `5001:5000` port mapping provided by this repository's `docker-compose.yml` file
since port 5000 is already used by macOS's `ControlCenter`!

### Step 5: Create a webcam on mainsail

Using mainsil's web UI, create a new web cam using the following settings:

- Name: Wyze Cam
- URL Stream: `http://WB_IP:8889/camera_name/` 
- URL Snapshot: `http://WB_USERNAME:WB_PASSWORD@WB_IP:5001/snapshot/camera_name.jpg`
- Service: WebRTC (MediaMTX)

Change the `camera_name` to the actual camera name from your Wyze account.

You can also access the Web-UI through `http://WB_IP:5001`
