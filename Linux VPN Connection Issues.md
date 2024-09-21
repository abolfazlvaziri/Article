# Linux VPN Connection Issues

![VPN](https://github.com/user-attachments/assets/7745ce7a-1ebc-4a72-a2aa-7f6911ed43ab)

One of the common issues faced by Linux users when using a VPN is the inability to connect or frequent disconnections. This issue is often caused by incorrect DNS settings. Changing the DNS to more reliable and faster servers can help resolve this problem.

# Changing System DNS
One of the quickest ways to resolve VPN connection problems is to modify the system’s DNS settings. Here are two methods you can use:

**Method 1:** 
1. Go to Settings. 
2. Navigate to Wi-Fi or your connected network. 
3. Under the IPv4 settings, disable automatic DNS. 
4. Enter either Google’s DNS (8.8.8.8) or Cisco’s DNS (208.67.220.220).

![VPN2](https://github.com/user-attachments/assets/c984c31f-bb0e-4e5c-bfa7-42645ac0c72d)

**Method 2 (Temporary):** 
1. Open the terminal and run the following command:
```sh
sudo vi /etc/resolv.conf
```

2. In the opened file, manually enter Google’s DNS (8.8.8.8) or Cisco’s DNS (208.67.220.220).

# Changing Application DNS
To fix VPN issues directly within your VPN applications, such as [Nekoray](https://github.com/MatsuriDayo/nekoray) or [Hiddify](https://github.com/hiddify/hiddify-next), follow these steps: 
1. Open the application’s settings. 
2. Locate the DNS settings and input Google’s DNS (8.8.8.8) or Cisco’s DNS (208.67.220.220).

![VPN3](https://github.com/user-attachments/assets/19d5facb-082e-4ab3-85dd-ee46929ea0d3)

# FoxyProxy Browser Extension | Without Tun Mode
If you are using a browser proxy, FoxyProxy is a convenient extension available for both Chrome and Firefox.

- [Download FoxyProxy for Chrome](https://chromewebstore.google.com/detail/foxyproxy/gcknhkkoolaabfmlnjonogaaifnjlfnp?hl=en)
- [Download FoxyProxy for Firefox](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/)

**To configure the VPN proxy settings:** 
1. Open FoxyProxy Setting > Proxies 
2. Set the title to “VPN-Nekoray” 
3. Enter Hostname = “127.0.0.1" and Port = “2080”(Nekoray Port).

- With this method, there is no need to use **Tun Mode** of VPN application, just open VPN application and select “VPN-Nekoray” proxy on FoxyProxy.

# Using Telegram Proxy
To configure a proxy for Telegram via Nekoray: 
1. Add a new proxy. 
2. Select “SOCKS5" as the proxy type. 
3. Set Hostname = “127.0.0.1” and Port = “2080”(Nekoray Port).

By following these methods, you can effectively troubleshoot and resolve VPN connectivity issues on Linux, ensuring a more stable and secure connection.

GitHub: [https://github.com/abolfazlvaziri](https://github.com/abolfazlvaziri) 
Instagram: [https://instagram.com/abolfazlvaziriofficial](https://instagram.com/abolfazlvaziriofficial) 
Telegram Channel: [https://t.me/AVN_COMMUNITY](https://t.me/AVN_COMMUNITY) 
YouTube: [https://www.youtube.com/@abolfazlvaziri](https://www.youtube.com/@abolfazlvaziri)
