
## Get Started


Clone this repo:
```sh
git clone https://github.com/tryptech/reddit-place-script-2023 && cd reddit-place-script-2023
```

Download our default config as `config.son`;
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

## Run the Script

### Windows

```shell
start.bat or startverbose.bat
```

### Unix-like (Linux, macOS etc.)

```shell
chmod +x start.sh startverbose.sh
./start.sh or ./startverbose.sh
```

