# STeMcell

Generally, here are some notes and/or tips about using a STeMcell

If you are interested in learning more from the creator, you can do so here:
* https://megamind4089.github.io/STeMCell/
* https://github.com/megamind4089/STeMCell

# Firmware

At the time of this writing (2022-06-06), you should use the stemcell_v1.0.1_plus branch ( https://github.com/sadekbaroudi/qmk_firmware/tree/stemcell_v1.0.1_plus ) to build your firmware for the STeMcell. Once the STeMcell is fully supported and tested for fingerpunch boards, I'll update master and will delete this branch.

In order to make STeMcell compatible firmware from this branch, you simply need to run the make command you would normally run, but add "STMC=yes" to the end. This will generate a uf2 file, which you will use to flash it.

## One time build

Starting with no repository, here's an example of building stock ffkb firmware with rgbmatrix and ec11 encoders:
```bash
# Cloning the repo
git clone git@github.com:sadekbaroudi/qmk_firmware.git
cd qmk_firmware
git submodule update --init --recursive
# Checking out the stemcell repo
git checkout -b master origin/master
git submodule update
# Running the firmware build
make fingerpunch/ffkb_byomcu/rgbmatrix_ec11:default CONVERT_TO=stemcell

# Build firmware will be .build/fingerpunch_ffkb_byomcu_rgbmatrix_ec11_default.uf2
# Copy it somewhere where you can drag and drop it to flash your STeMcell
```

## Managing fingerpunch code in your repository

Now, you may ask yourself the question: "But how do I organize and manage my code in there?"

My recommendation is to add my repo as a remote repository to yours. So, instead of cloning my repo, add it to yours using "git remote"

Note that you can do the below multiple times for different keyboards by replacing the branch name ffkb with whatever you like. You will not need to repeat the "git remote add" command once you have run it the first time.

```bash
# Add the fingerpunch repo as a remote repository
git remote add fingerpunch git@github.com:sadekbaroudi/qmk_firmware.git
git fetch fingerpunch

# Checkout the master branch as a branch that tracks your keyboard. Let's use ffkb in this example
git checkout -b ffkb fingerpunch/master
git submodule update

# add your keymap, code, etc into keyboards/fingerpunch/ffkb_byomcu/keymaps/{your_keymap_dir}

# commit your changes
git commit -a -m "Your commit message"

# push the branch to your repo
git push origin ffkb

# If you ever want to update from the latest fingerpunch stemcell branch in the future
git fetch fingerpunch
git merge fingerpunch/master
git push origin ffkb
```

# Flashing

## Resetting the controller for flashing

You have a few options for putting the STeMcell into flashing mode
* Short RST and GND pins on the controller itself
* Double tap the physical reset switch on the keyboard pcb itself
* Use the QMK reset keycode

## Flashing the firmware itself

Given this is using the tinyuf2 bootloader, flashing is VERY simple. When you go into flashing mode, a window will pop up on your machine (like putting a USB drive into your machine). All you need to do is copy the .uf2 file into the root directory, and you're done!

# Soldering

## STeMcell Jumpers

The first thing you should do is solder the jumpers on the back of the STeMcell, as shown in the image below.

![STeMcell jumpers](images/stemcell-jumpers.jpg)

# Testing

Before soldering the controller onto your pcb, you may want to test that you soldered the jumpers correctly. If so, see this:
https://github.com/sadekbaroudi/fingerpunch/blob/master/stemcell/JUMPER_TESTER.md
