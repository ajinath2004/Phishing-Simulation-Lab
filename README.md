<img width="1920" height="920" alt="Screenshot_2026-10-15_12_03_05" src="https://github.com/user-attachments/assets/6e2abbf1-917d-4ea4-bb4b-244f07eb4b59" /># Phishing-Simulation-Lab
A hands-on lab showing how Zphisher generates fake login pages and captures credentials. Built to study phishing mechanics and understand defensive steps like MFA.
- Zphisher is an open-source social engineering tool used by penetration testers and security researchers to simulate phishing attacks. It automates the process of generating fake login
- pages (clones) for popular platforms such as social media sites, email providers, and cloud services.
- In a security testing or awareness scenario, it allows testers to demonstrate how easily users can be misled by deceptive URLs and spoofed landing pages, highlighting the human element in - cybersecurity risks.

## Installation & Setup

- To install Zphisher, I cloned the repository directly from GitHub and navigated into the project directory
- git clone https://github.com/htr-tech/zphisher.git
- cd zphisher
- sudo bash zphisher.sh
- sudo bash zphisher.sh to run the script with root access, which is needed for setting up local network services and launching the interface properly

## Step 2: Selecting the Target Template
- After launching the tool, the main menu asks me to choose which service's login page I want to simulate. 

- <img width="1920" height="920" alt="Screenshot_2026-10-15_12_02_57" src="https://github.com/user-attachments/assets/96df04e6-0536-495b-aa50-3c777f963f57" />

* In this step, I entered `03` to select **Google** cite 
* Choosing this option tells Zphisher to load the pre-built Google fake login template so it can host a replica of the Google sign-in page for the testing lab.

<img width="1920" height="920" alt="Screenshot_2026-10-15_12_03_05" src="https://github.com/user-attachments/assets/28591ff9-5ab9-48c1-93d1-6558dc6bc6a6" />

- ## Step 3: Choosing the Specific Sub-Option
- Once Google is selected, Zphisher displays specific template variations for the attack vector[cite: 3]:

- <img width="1920" height="920" alt="Screenshot_2026-10-15_12_03_48" src="https://github.com/user-attachments/assets/b81150ca-d474-410b-abbb-2b18315f9e4f" />

- 01: Gmail Old Login Page
- 02: Gmail New Login Page
- 03: Advanced Voting Poll

- For this lab, I chose option `0` (advance voting poll ) to test the modern interface clone that most target users are familiar with.

- ## Step 4: Port Forwarding / Hosting Option

Next, Zphisher prompts to select a port forwarding or hosting method to serve the phishing page

- 01:Localhost (Hosts the page locally on the machine)
- 02: Cloudflared (Creates a Cloudflare tunnel for external access)
- 03: LocalXpose (Alternative tunneling service)
- Since this test was conducted strictly inside a controlled local lab environment, I selected option `01` (Localhost) to ensure the service remains restricted to the local network

## Step 5: Server Started & Listener Active

- <img width="1920" height="920" alt="Screenshot_2026-10-15_12_04_36" src="https://github.com/user-attachments/assets/a94496d8-15bc-4b16-bb93-dee3c4f6b781" />
- The phishing page is now hosted locally at `http://127.0.0.1:8080` The tool is actively listening and waiting for any user interaction or login attempt to capture credentials in real 
- time 

- <img width="1920" height="920" alt="Screenshot_2026-10-15_12_05_21" src="https://github.com/user-attachments/assets/9a1d8760-5a68-46c9-8e57-59483597acfd" />

- ## Step 5: Port Configuration & Server Setup

- In this step, I chose to set a custom port by typing `y` and specified port `8080`[cite: 6]. Zphisher then initialized the local URL (`http://127.0.0.1:8080`) and started the internal
- PHP server to host the page

## Step 6: Exposing Localhost via Cloudflared Tunnel

- <img width="1920" height="920" alt="image" src="https://github.com/user-attachments/assets/ee3ed89b-797b-4b8c-ba12-d1ed9bb6f142" />

- Since the phishing page is running locally on port 8080, I opened a second terminal tab to create a public tunnel using **Cloudflared**

``bash
cloudflared tunnel --url http://localhost:8080 ```

## Step 7: Tunnel Link Generation & Target Delivery

<img width="1920" height="920" alt="Screenshot_2026-10-15_12_07_23" src="https://github.com/user-attachments/assets/f2c01fbd-2363-4397-afdd-187977dba222" />

- Once Cloudflared initializes, it generates a public URL linked directly to the local phishing server
- Generated URL:** `https://anticipated-rid-clearly-blend.trycloudflare.com`
- Delivery:** In a real social engineering simulation, this public HTTPS link is sent to the target user (e.g., via email, SMS, or messaging apps) to test their awareness against deceptive - links.

- When the target opens this link in their browser, they are automatically redirected to the fake Google login page hosted on our local machine


## Step 8: Target Interaction & Credential Submission

- Once the target user accesses the generated link, they are directed to a cloned login page (in this case, a voting poll verification page)

- <img width="1904" height="915" alt="Screenshot 2026-10-15 224832" src="https://github.com/user-attachments/assets/f4050aca-3de9-4281-89ed-67146f1dc3df" />
- <img width="1904" height="957" alt="Screenshot 2026-10-15 213933" src="https://github.com/user-attachments/assets/e2cf855b-6066-4c89-b427-61c71417c35b" />

- User Action: The victim enters their account credentials (e.g., `project@gmail.com`) and password into the spoofed interface and clicks **Login**
- Observation: The interface mimics a legitimate authentication flow, tricking the user into submitting sensitive input on an unverified domain (`trycloudflare.com`)
- Key Takeaway: This demonstrates the importance of checking URL domain names and relying on security indicators like domain reputation before entering credentials.

- ## Step 9: Captured Credentials & Attack Analysis

- As soon as the target user submits their details on the fake page, the active listener in Zphisher captures the network data and displays it live in the terminal[cite: 10]:

- <img width="1920" height="920" alt="Screenshot_2026-10-15_12_10_17" src="https://github.com/user-attachments/assets/2ef02a1e-52dd-462c-934e-91742ca76450" />


- Victim IP Captured: The tool logged the IP address (`103.230.150.208`) as soon as the victim accessed the page and saved it inside `auth/ip.txt`
- Credentials Captured: The submitted username (`project@gmail.com`) and password (`projectforgithub`) were successfully intercepted and saved inside `auth/usernames.dat`.
- Real-time Logging: The terminal immediately updates to show that login info was found and continues to listen for next attempts

> **Defensive Takeaways & Mitigation**
> 
> This lab highlights how convincing phishing attacks can be. To protect against these threats:
> * **Verify the URL:** Always check the browser address bar for the exact domain name. Never enter credentials on unrecognized subdomains or tunneling services (like `trycloudflare.com`).
> * **Enable Multi-Factor Authentication (MFA):** Using hardware keys or app-based authenticators ensures that stolen passwords alone aren't enough to compromise an account.
> * **Rely on Password Managers:** Password managers auto-fill credentials only on matched domains, preventing auto-submission on fake phishing pages.
> * **Security Awareness:** Recognize urgent language, spoofed senders, and suspicious links before interacting with login prompts.
