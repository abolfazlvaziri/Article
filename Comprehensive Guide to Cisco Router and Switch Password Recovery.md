![getimg_ai_img-LBLGF0kWn2PdB6nyPLWpL](https://github.com/user-attachments/assets/8beb9cf4-13b0-49cc-a4ac-2717dad3a44f)

Forgotten passwords can disrupt even the most organized networks. This guide equips you with proven methods to recover access on Cisco routers and switches efficiently, safeguarding your configurations while restoring control.

---
### Password Recovery for Cisco Routers
#### Prerequisites:
- Ensure a console connection is established with the router using a PC.
#### Steps:
1. **Connect to Console Port**: Use a terminal application like PuTTY or Mobaxterm to connect to the router via the console port.
2. **Power Cycle the Device**: Turn off the router and then turn it back on.
3. **Interrupt Boot Process**: Hold down the `MODE` button as the router powers on. Keep it pressed for 10–15 seconds.
4. **Bypass Startup Configuration, Enter the following commands:**
 ```sh
confreg 0x2142
boot
```
- This bypasses the `NVRAM` and loads the device as if it were new, without any saved passwords.
5. **Access Configuration Mode, After the device boots**:
```sh
enable
copy startup-config running-config
conf t
no enable secret
[Other Command]
config-register 0x2102
do write
reload
```
6. **Verify Configuration:**
   - Use the show boot command to confirm changes.
   - Ensure the device boots into normal mode with the proper configuration.

**Note**: This method can also be applied for recovering Telnet, SSH, or other passwords.

---
### Password Recovery for Cisco Switches (Old Version Switches)
#### Steps:
1. **Connect to Console Port:** Connect to the switch using a console connection.
2. **Power Cycle:** Disconnect and reconnect the power cable.
3. **Interrupt Boot Process:** Hold down the `MODE` button for 10–15 seconds during boot.
4. **Enter `ROMMON` Mode, Run these commands:**
```sh
flash_init
rename flash:config.text flash:config.old
boot
```
- Allow the switch to boot fully (it will start as a fresh device).
5. **Restore Configuration, Rename the configuration back:
```sh
rename flash:config.old flash:config.text
copy flash:config.text running-config
conf t
no username [username]
no enable secret
do write
```
- **If errors occur during this process, use:**
```sh
enable
conf t
line 0
logging synchronous
```
- Ensures system log messages are displayed in a synchronized manner, preventing interruptions to command input.
#### New Version Switches
#### Steps:
1. **Connect to Console Port:** Establish a connection via console.
2. **Power Cycle and Interrupt Boot:** Disconnect and reconnect the power cable, hold the `MODE` button for 10–15 seconds during boot.
3. **Modify Boot Parameters, Enter commands in `ROMMON` Mode:**
```sh
flash_init
SWITCH_IGNORE_STARTUP_CFG=1
boot flash:packages.conf
```
 4. **Recover Configuration, once booted:**
```sh
copy startup-config running-config
[CHANGE_PASSWORD] or [REMOVE PASSWORD]
copy running-config startup-config
reload
```
 5. **Restore Boot Mode:**
   - Hold the `MODE` button again to enter boot mode, execute this commands:
```sh
SWITCH_IGNORE_STARTUP_CFG=0
boot flash:packages.conf
```
 6. **Finalize, allow the switch to boot fully and disable manual boot:**
 ```sh
no boot manual
```

**Note**: In some cases, it may be necessary to repeat the process multiple times to achieve successful recovery.

----

Medium: https://medium.com/@abolfazl.vaziri
Instagram: [https://instagram.com/abolfazlvaziriofficial](https://instagram.com/abolfazlvaziriofficial]%28https://instagram.com/abolfazlvaziriofficial%29)  
Telegram Channel: [https://t.me/AVN_COMMUNITY](https://t.me/AVN_COMMUNITY]%28https://t.me/AVN_COMMUNITY%29)  
YouTube: [https://www.youtube.com/@abolfazlvaziri](https://www.youtube.com/@abolfazlvaziri]%28https://www.youtube.com/@abolfazlvaziri%29)  
Linkedin: [https://www.linkedin.com/in/abolfazlvaziri1](https://www.linkedin.com/in/abolfazlvaziri1/)






