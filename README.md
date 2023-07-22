
## Requirements

- [Python 3.10](https://www.python.org/downloads/)
- [fork reddit-place-script-2023](https://github.com/tryptech/reddit-place-script-2023) (see instructions for installation bellow)
## Get Started


Clone this repo:
```sh
git clone https://github.com/tryptech/reddit-place-script-2023 && cd reddit-place-script-2023
```

Download our default config as `config.json`;
```sh
curl https://raw.githubusercontent.com/ggjulio/rplace42/main/config_example.json > config.json
```

Edit the values to replace with actual credentials and values
Note: Please use <https://jsonlint.com/> to check that your JSON file is correctly formatted

```json
{
    // The URLs to the template overlays
    "template_urls": [
        "https://url.to.the.template1.png",
        "https://url.to.the.template2.png"
    ],
    // The URL to a priority template with a filtered list of sources from a template overlay from "template_urls"
    "priority_url": "https://url.to.the.template3.png",
    // Filter only templates with names in this list, if empty take all
    "names": ["dont-panic", "whale", "marvin head", "norminet"],
    //Where the template image will be saved or loaded from
    "image_path": "image.png",
    // delay between starting threads (can be 0)
    "thread_delay": 2,
    // array of accounts to use
    "workers": {
        // username of account 1
        "worker1username": {
            // password of account 1
            "password": "password",
        },
        // username of account 2
        "worker1username": {
            // password of account 2
            "password": "password",
        }
        // etc... add as many accounts as you want (but reddit may detect you the more you add)
    }
}
```
Run the Application

- Windows
	```shell
	start.bat # or startverbose.bat
	```
- Unix-like (Linux, macOS etc.)
	```shell
	chmod +x start.sh startverbose.sh
	```
	```sh
	./start.sh # or ./startverbose.sh
	```
- Via docker
	After editing the `config.json` file, run the following commands
	```sh
	docker build -t place-bot .
	```
	```sh
	docker run place-bot
	```

## Multiple Workers

Just create multiple child arrays to "workers" in the .json file:

```json
{
    "image_path": "image.png",
    "thread_delay": 2,

    "workers": {
        "worker1username": {
            "password": "password",
        },
        "worker2username": {
            "password": "password",
        }
    }
}
```

In this case, both workers will draw random pixels from the input image file.

This is useful if you want different threads drawing different parts of the image with different accounts.


## Other Settings

If any JSON decoders errors are found, the `config.json` needs to be fixed. Make sure to add the below 2 lines in the file.

```json
{
    "thread_delay": 2,
    "unverified_rate_limit": false,
    "proxies": ["1.1.1.1:8080", "2.2.2.2:1234"],
    "compact_logging": true
}
```

- thread_delay - Adds a delay between starting a new thread. Can be used to avoid ratelimiting.
- unverified_rate_limit - Sets the pixel place frequency to the unverified account limit.
- proxies - Sets proxies to use for sending requests to reddit. The proxy used is randomly selected for each request. Can be used to avoid ratelimiting.
- compact_logging - Disables timer text until next pixel.
- Transparency can be achieved by using the RGB value (69, 42, 0) in any part of your image.
- If you'd like, you can enable Verbose Mode by adding `--verbose` to "python main.py". This will output a lot more information, and not necessarily in the right order, but it is useful for development and debugging.
- You can also setup proxies by creating a "proxies" and have a new line for each proxies.

# Tor

Tor can be used as an alternative to normal proxies. Note that currently, you cannot use normal proxies and tor at the same time.

```json
"using_tor": false,
"tor_ip": "127.0.0.1",
"tor_port": 1881,
"tor_control_port": 9051,
"tor_password": "Passwort",
"tor_delay": 5,
"use_builtin_tor": true
```

The config values are as follows:

- Deactivates or activates tor.
- Sets the ip/hostname of the tor proxy to use
- Sets the httptunnel port that should be used.
- Sets the tor control port.
- Sets the password. (Leave it as "Passwort" if you want to use the default binaries.)
- The delay that tor should receive to process a new connection.
- Whether the included tor binary should be used. It is preconfigured. If you want to use your own binary, make sure you configure it properly.

Note that when using the included binaries, only the tunnel port is explicitly set while starting tor.

<h3>If you want to use your own binaries, follow these steps:</h3>

- Get tor standalone for your platform [here](https://www.torproject.org/download/tor/). For Windows just use the expert bundle. For macOS, you can use [Homebrew](https://brew.sh) to install tor: ``brew install tor``.
- In your tor folder, create a file named ``torrc``. Copy [this](https://github.com/torproject/tor/blob/main/src/config/torrc.sample.in) into it.
- Search for ``ControlPort`` in your torrc file and uncomment it. Change the port number to your desired control port.
- Decide on the password you want to use. Run ``tor --hash-password PASSWORD`` from a terminal in the folder with your tor executable, with "PASSWORD" being your desired password. Copy the resulting hash.
- Search for ``HashedControlPassword`` and uncomment it. Paste the hash value you copied after it.
- Decide on a port for your httptunnel. The default for this script is 1881.
- Fill in your password, your httptunnel port and your control port in this script's ``config.json`` and enable tor with ``using_tor = true``.
- To start tor, run ``tor --defaults-torrc PATHTOTORRC --HttpTunnelPort TUNNELPORT``, with PATHTOTORRC being your path to the torrc file you created and TUNNELPORT being your httptunnel port.
- Now run the script and (hopefully) everything should work.
