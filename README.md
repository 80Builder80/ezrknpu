# ezrknpu
Easy usage of Rockchip's NPU found in RK3588 and similar chips. This includes ChatGPT-like LLMs and models like YoloV5. 
This repo is divided in two submodules:
- https://github.com/Pelochus/ezrknn-llm
- https://github.com/Pelochus/ezrknn-toolkit2

Apart from that, you can find converted LLMs (and how to download them) here:
- https://huggingface.co/Pelochus/ezrkllm-collection

Currently focusing only on RK3588 and RK3588S.

## Requirements
Keep in mind this repo is focused for:
- Installing what's needed for Linux and running on the SBC directly.
- Rockchip RK3588, but I accept contributions for running on other compatible Rockchip SoCs.
- Using NPU driver 0.9.6 or higher (for basically any LLM except the smallest like Qwen 1.8). Check it with `dmesg | grep -i rknpu`
- You are running either Debian 11 or Ubuntu 22.04 or later versions. Derivatives should possibly work fine too. I personally **recommend either Armbian or Ubuntu Rockchip**

## Recommended OS
Use this OS for now:
https://github.com/Pelochus/armbian-build-rknpu-0.9.6

This is a modified Armbian by me with the NPU driver 0.9.6. 
I've made a PR (already approved) to the Armbian guys to include the driver 0.9.6, in future releases with kernel 6.1, Rockchip devices will already have this driver included from the official Armbian guys. 

## Quick Install
This will install required dependencies, packages, libraries and install both RKNN Toolkit 2 and RKNN LLM.

Run: 
```bash
curl https://raw.githubusercontent.com/Pelochus/ezrknpu/main/install.sh | sudo bash
```

## Custom install
You can download the `install.sh` script in this repo and run it with:
- `-llm`: for installing only RKNN LLM
- `-nn`: for installing only RKNN Toolkit 2

## Running
Depending on what you want to run, please refer to these links:

### Test Run
Currently, the lightest (works on 4GB of RAM) and best working model is Qwen 1.8B Chat. To run it:

```bash
GIT_LFS_SKIP_SMUDGE=1 git clone https://huggingface.co/Pelochus/qwen-1_8B-rk3588 # Running git lfs pull after is usually better
cd qwen-1_8B-rk3588 && git lfs pull # Pull model
rkllm qwen-chat-1_8B.rkllm # Run!
```

Wait for about a minute before the model loads.
If something fails, perhaps it is a good idea keep reading below.

### Downloading and running a LLM on your NPU
For downloading:
- https://huggingface.co/Pelochus/ezrkllm-collection

For running an already downloaded model:
- https://github.com/Pelochus/ezrknn-llm?tab=readme-ov-file#test

Converting a compatible model to RKLLM format (check previously Rockchip's docs on rknn-llm repo). This needs a x86 PC:
- https://github.com/Pelochus/ezrknn-llm?tab=readme-ov-file#converting-llms-for-rockchips-npus

### Running and using the RKNN-Toolkit for NN applications
https://github.com/Pelochus/ezrknn-toolkit2/?tab=readme-ov-file#test

<hr>

## Checking NPU info
### NPU usage
You have 3 options for checking usage:
- rknputop (graphical option, works on OSes without GUI): https://github.com/ramonbroox/rknputop
- Running `ntop.sh`, from this repo which runs the command from next point every 0.5 s
- Running `sudo cat /sys/kernel/debug/rknpu/load`

### NPU driver
Run `dmesg | grep -i rknpu` and it should output something like:

```bash
[    7.648610] RKNPU fdab0000.npu: Adding to iommu group 0
[    7.648747] RKNPU fdab0000.npu: RKNPU: rknpu iommu is enabled, using iommu mode
[    7.648893] RKNPU fdab0000.npu: Looking up rknpu-supply from device tree
[    7.650056] RKNPU fdab0000.npu: Looking up mem-supply from device tree
[    7.652808] RKNPU fdab0000.npu: can't request region for resource [mem 0xfdab0000-0xfdabffff]
[    7.652838] RKNPU fdab0000.npu: can't request region for resource [mem 0xfdac0000-0xfdacffff]
[    7.652859] RKNPU fdab0000.npu: can't request region for resource [mem 0xfdad0000-0xfdadffff]
[    7.653197] [drm] Initialized rknpu 0.9.5 20240226 for fdab0000.npu on minor 1
```

## Useful Links
- Dedicated subreddit. I **strongly recommend** taking a look at the wiki and various posts there: https://www.reddit.com/r/RockchipNPU/
- LLM submodule for this repo: https://github.com/Pelochus/ezrknn-llm
- NN submodule for this repo: https://github.com/Pelochus/ezrknn-toolkit2

## Contributing
Please open an issue or PR on the corresponding submodule repo. If unsure, open it on this repo:
- For RKLLM related: https://github.com/Pelochus/ezrknn-llm
- For RKNN related: https://github.com/Pelochus/ezrknn-toolkit2

Currently (and mainly) there are the following contributions to be made:
- [ ] Other Rockchip SoCs support. Check their documentation to see which are capable.
- [ ] Android support
- [x] Conversion of other compatible LLMs (only some variations of Qwen 1.5 left, not doing it for now)
- [ ] Improve documentation
- [ ] Other general improvements to installation scripts, Docker containers, support for other Linux distros like Arch, submitting issues, adding more error checking...

## Credits
To the r/RockchipNPU subreddit, which helped me tremendously with testing and discovering why things failed and how to solve those issues.
