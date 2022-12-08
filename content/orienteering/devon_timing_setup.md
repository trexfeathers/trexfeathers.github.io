## Devon Orienteering's timing setup

DEVON uses the 
[SPORTident](https://www.sportident.com) timing system, supplied by
[SPORTident UK](https://www.sportident.co.uk). Many 
thanks to SPORTident UK; their 
[SiTiming](https://www.sportident.co.uk/software/sitiming) 
software is superb for staging events, and they are unfailingly 
supportive, even during my ambitious misadventures trying new ideas!

### Controls

[Detailed here](https://www.devonorienteering.co.uk/devon-oc-planner-si-guidance)

Kites and stakes are bundled in 10's, with the expected total number clearly 
visible on their bags/boxes - to encourage counting out and counting back in.

#### SiAC

The stations are always SiAC-enabled. This gives all event Planners the 
option of supporting SiAC for their event, without any intervention from Timing 
volunteers (more effort, and opportunity for mis-communication). This 
benefit is considered worth the extra battery drain.

[Clear instructions](https://www.devonorienteering.co.uk/organisers-resources)
are provided on how to advertise the event regarding whether contactless
punching is guaranteed. Can get confusing otherwise, as later starters get 
contactless runs even at events where SiAC isn't explicitly supported.

The Start is not SiAC-enabled - experience shows this causes confusion among 
start volunteers.

The Finish is not SiAC-enabled - it is a valuable to be able to check this 
station for punches in case of a non-downloaded competitor, and contactless 
punches are not stored on stations.

### Download

Specific sections below. Details on using this setup at an event are in 
[DEVON's SiTiming Guide](https://www.devonorienteering.co.uk/devon-oc-si-timing-guide).

### Download: Power

Krisdonia 25000mAh and 50000mAh power banks.

Sold under many names,
[here's the site for the mainland Europe version](https://www.litionite.com/product/tanker/).

Power banks are much safer, smaller and simpler than the traditional 12V 
battery + inverter setup. However: no mains power influences choice of all other 
download equipment - see sections below.

The Krisdonias were chosen specifically for their wide array of DC power 
adapters - can plug into any laptop we use. We don't need a bank per laptop - 
laptops can last some time on their own,
so we swap power banks around as different laptops get low batteries.

### Download: Laptops

We use refurbished laptops, preferably ex-business laptops, which 
tend to be more reliable. Requirements:

- Solid state drive - massively improves performance. Can have minimal
size - just enough for Windows and not much more.
- `> 4GB` RAM.
- `> 2hr` battery.

A common Microsoft account between all the laptops, including OneDrive, 
makes it easier to keep the laptop files and settings aligned with each other.

Thanks to [Mike's Laptops](https://www.mikeslaptops.co.uk/) of Exeter for 
supplying and maintaining our club laptops.

### Download: Printers

[Unitech SP319](https://portal.unitech.eu/Products/Default.aspx?RFID=1&PG=137)

Chosen because power and print jobs both come via the same USB cable.

#### Tips for optimal performance

- SiTiming `v4357`
([release notes](https://www.sportident.co.uk/software/sitiming/sitiming_release_notes.php))
includes an enhancement developed with DEVON to avoid printouts having long
blank 'tails'. Make sure you
[select the shortest available paper size](https://gb-kb.sage.com/portal/app/portlets/results/viewsolution.jsp?solutionid=200427112156400).
- Use a high quality 3rd party USB-C cable - the stock cable does not seat 
securely in the printer, causing frequent drop-outs. We have used
[this StarTech cable from RS Components](https://uk.rs-online.com/web/p/usb-cables/2133133).
- Keep the printer clean of dust, especially the roller. We use
[this Phoenix cleaning pen from RS Components](https://uk.rs-online.com/web/p/label-printer-accessories/6618988).

### Download: Networking

[Cable Matters USB ethernet switcher](https://www.cablematters.com/pc-1273-138-usb-31-to-4-port-gigabit-ethernet-adapter.aspx)

Ethernet minimises latency, which is important when running download and the 
database on separate laptops.

We can use Wi-Fi in an emergency - any laptop can create a network using the 
below script:

```shell
@echo off
echo SETTING UP LOCAL WIRELESS NETWORK...
@echo on

netsh wlan set hostednetwork mode=allow ssid=%COMPUTERNAME% key=my-password
netsh wlan start hostednetwork

@echo off
timeout 10
@echo on
```

### Download: Displaying Results

No mains power prevents using an A4 printer or big screens.

Basic results can be printed by the splits printers. We offer full results 
pages via attendee's smartphones, with basic tablet computers for those that 
aren't carrying phones.

#### Tablet computers

Kindle Fire or any other budget tablet

As basic as possible - just need Wi-Fi and a web browser. Low power
requirements, and can charge from the power banks if needed.

#### Live results to website (smartphone Wi-Fi hotspot)

Ideally via smartphone hotspot and FTP onto the club website. Attendees 
get results by visiting the club website on their personal smartphones, or on 
the tablet computers which access the website via the smartphone hotspot.

Need a website that supports FTP uploads from SiTiming's `HTML Results` 
interface. DEVON's website is provided by [PFweb](http://www.pfweb.co.uk/) 
and includes an effective setup for this.

#### Live results locally (USB Wi-Fi router)

[GL.iNet Mango (GL-MT300N-V2)](https://www.gl-inet.com/products/gl-mt300n-v2/)

For when a smartphone can't be used (e.g. signal, mobile data limits). 
Attendees' personal smartphones and tablets both connect to the router and 
visit the results webpage - details of both these steps are described in the
Live Results appendix of
[DEVON's SiTiming Guide](https://www.devonorienteering.co.uk/devon-oc-si-timing-guide).

This router is fantastic. It runs on USB power, has
[comprehensive documentation](https://docs.gl-inet.com/en/3/setup/mini_router/first_time_setup/),
and uses open source 
software so can be configured as an FTP server that will receive and then 
display SiTiming's `HTML Results` output.

Here's how we did it:

##### Router basic setup

- Connected router to the Internet - ethernet cable into the `WAN` socket, 
and into one of the router network sockets.
- Connected to the router from a PC:
  - Connect to the router's Wi-Fi network using its default settings.
  - Visit `http://192.168.8.1` in any web browser.
  - WHENEVER YOU CONNECT: I recommend going via a private/incognito window. 
This avoids some redirect mess that I got into.
  - Now is the time to customise the admin password, Wi-Fi name and Wi-Fi 
password.
- Updated the router to the latest firmware.
  - It can do this automatically, but in my case it was so out of date that I
needed to download the update from their website then upload onto the
router - they have a menu for this.
- Change the `LAN IP` (under `More Settings`) to `10.10.10.10` - easier for 
attendees to enter versus the standard IP.
- Installed `vsftpd` - a simple FTP server - via the `Plugins` menu.

##### Router FTP server configuration

- Connected to router via WinSCP -
[instructions here](https://docs.gl-inet.com/en/3/tutorials/scp/).
- Made new directories for the results:
  - `/usr/results`
  - `/usr/results/css`
  - `/usr/results/public`
    - Set this one to have read/write permissions for User, Group and Others.
    - If you ever put something in this directory manually (not via FTP), 
make sure it has these permissions too else FTP results update may break.
  - Make a link: `/usr/results/public/css`, which links to `/usr/results/css`.
- Used the router's existing website hosting to make the results page(s) 
visible in browsers:
  - Make a link: `/www/results`, which links to `/usr/results/public`.
  - Make a link: `/usr/results/public/idx_vue.html`, which links to
`index.html`.
- Copied SiTimings Override CSS files into `/usr/results/css`.
  - See the SiTiming User Guide for where to find these files.
- Started `vsftpd`, and enabled auto-start. 
  - Used the commands listed
[here](https://openwrt.org/docs/guide-user/services/nas/ftp.overview)
(note: none of the other points on that page are relevant to our case).
  - These commands can be entered using WinSCP's **terminal**.
- Edited `/etc/vsftpd.conf` to set results directory editing to be done by 
anonymous login:

```
# Guidance: https://linux.die.net/man/5/vsftpd.conf
background=YES
listen=YES
check_shell=NO
anonymous_enable=YES
anon_root=/usr/live-results
write_enable=YES
anon_mkdir_write_enable=YES
anon_other_write_enable=YES
anon_upload_enable=YES
anon_umask=000
```

- `vsftpd` needed some 'anonymous user' directories (default behaviour 
presumably), which we had to create:
  - `/home`
  - `/home/ftp`
  - `/home/anonymous`
