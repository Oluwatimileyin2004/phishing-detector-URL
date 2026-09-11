# Phishing URL Detector

A beginner-friendly cybersecurity project built with Python that analyzes URLs for common phishing indicators and assigns a suspicion score based on the structure of the URL and the age of its domain.

## Project Overview

Phishing attacks often use deceptive URLs to trick users into visiting malicious websites or revealing sensitive information.

This project provides a simple way to analyze a URL before visiting it. It checks for suspicious characteristics such as:
- Use of a raw IP address instead of a domain name
- Excessive subdomains
- Hyphens in the brand name
- Suspicious keywords such as login, verify, and signin
- Newly registered domains

The detected indicators are combined to produce a Suspicion Score from 0 to 10.

Note: This project is for educational purposes and is not a replacement for professional phishing detection or security software.

## Features

- URL structure analysis
- Raw IP address detection
- Excessive subdomain detection
- Detection of hyphens in domain names
- Detection of suspicious authentication-related keywords
- Domain age checking using WHOIS
- Suspicion scoring from 0–10
- Low, Medium, and High Risk classification
- WHOIS timeout and error handling (optimized for network setups like Kali Linux)

## Technologies Used

- Python 3
- ipaddress
- datetime
- socket
- urllib.parse
- python-whois

## Project Structure

```text
phishing-url-detector/
├── phishing_url_detector.py
└── README.md
```

## Running the Project

1. Clone the repository:
```bash
git clone https://github.com/<YOUR-USERNAME>/phishing-url-detector.git
```

2. Navigate into the project directory:
```bash
cd phishing-url-detector
```

3. Install the required dependency:
```bash
pip install python-whois
```

4. Run the program:
```bash
python phishing_url_detector.py
```

The script contains sample URLs that can be used to test the detector. You can also modify the URLs inside the Python file and test other targets.

## Suspicion Scoring

The detector uses a simple rule-based scoring system:

**Indicators:**
- **Raw IP address instead of domain:** +2
- **Excessive subdomains (≥ 3 dots):** +3
- **Hyphen in brand name:** +3
- **`login`, `verify`, or `signin` detected:** +1
- **Domain less than 30 days old:** +4

**Risk Classification:**
- **0–3:** Low Risk 🟢
- **4–6:** Medium Risk 🟡
- **7–10:** High Risk 🔴

## How It Works

The detector performs two main types of analysis:

### URL Structure Analysis
The program parses the URL and examines its structure. It checks whether the URL uses an IP address instead of a domain, contains excessive subdomains, contains a hyphen in the brand portion, or contains authentication-related keywords. Each detected indicator contributes points to the suspicion score.

### Domain Age Analysis
The program uses WHOIS information to check the domain's creation date. If the domain is less than 30 days old, additional points are added to the suspicion score. If the WHOIS request times out or encounters an error, the program continues running safely without adding points.

## What I Learned

Building this project helped me develop both Python programming and cybersecurity skills as part of my studies at Olabisi Onabanjo University.

### Python Skills
I learned how to:
- Work with Python functions
- Use conditional statements and exception handling
- Import and use external Python libraries
- Parse URLs using `urllib.parse`
- Work with IP addresses using `ipaddress`
- Work with dates using `datetime`
- Handle network-related errors and timeouts
- Organize a Python program into separate functions
- Return and combine values from different functions

### Cybersecurity Skills
I learned how to:
- Identify common characteristics of phishing URLs
- Analyze URL structure as a security indicator
- Understand why attackers may use suspicious domains and subdomains
- Understand how domain age can be used as an indicator during URL analysis
- Develop a simple rule-based security detection system
- Assign risk scores based on multiple security indicators
- Recognize that security indicators do not always provide definitive conclusions

### Practical Experience
The project helped me understand how programming can be applied to cybersecurity and forensic investigations. Instead of studying phishing purely theoretically, I built a functional tool that applies core threat detection principles to live URL analysis.

## Limitations

Although the detector can identify several suspicious characteristics, it has key limitations:
- A legitimate website may trigger one or more suspicious indicators.
- A malicious website may avoid the checks used by the program.
- Domain age alone does not determine whether a website is malicious.
- WHOIS information may be unavailable or blocked.
- The program does not inspect the actual content of a website.
- It does not check external threat-intelligence databases (e.g., VirusTotal).
- It does not analyze website SSL/TLS certificates.
- It does not use machine learning models.
- The scoring system is a simple educational heuristic model.

Therefore, a **Low Risk** result does not guarantee that a URL is completely safe.

## How the Project Can Be Improved

There are several ways this project could be developed into a more advanced phishing detection system:

1. **Add More URL Indicators:** Introduce checks for URL shorteners, suspicious TLDs, extremely long paths, encoded characters, custom ports, and look-alike Unicode characters.
2. **Add Threat Intelligence Integration:** Connect to threat-intelligence services and URL reputation APIs to compare links against live blacklists.
3. **Improve Domain Analysis:** Expand lookup details to include DNS records, nameservers, IP reputation, and registrar history.
4. **Add SSL/TLS Inspection:** Check HTTPS validation and inspect SSL certificate issuing authorities.
5. **Create a Web or Graphical Interface:** Develop a Streamlit web app or GUI for non-technical users to enter links and view visual risk breakdowns.
6. **Add Machine Learning:** Train classifiers (such as Random Forest) on large URL datasets to identify subtle phishing patterns.
7. **Generate Detailed Reports:** Export structured scan summaries in JSON or PDF formats.

## Ethical Use

This project was created for educational, research, and cybersecurity learning purposes. It should be used responsibly and strictly for non-malicious defensive research.

## Future Goals

My goal is to continue refining this project by adding machine learning classification models, threat-intelligence API integrations, and an interactive interface.
import requests

KNOWN_SHORTENERS = ["bit.ly", "tinyurl.com", "t.co", "is.gd", "goo.gl", "ow.ly"]

def unshorten_url(url):
    """Traces HTTP redirects to extract the final landing URL."""
    try:
        # Use HEAD request so we don't download the entire web page body
        response = requests.head(url, allow_redirects=True, timeout=5)
        final_url = response.url
        
        if final_url != url:
            return final_url, f"⚠️ Shortened URL detected! Resolved to: {final_url}"
        return url, "No URL shortener detected."
    except requests.RequestException:
        return url, "Could not follow redirect (Connection failed)."
## Author

**Oguntola Timileyin Emmanuel**  
*Criminology & Security Studies Student | Olabisi Onabanjo University (OOU)*  
*Focus: Cybercrime Investigation, Forensic Science & Threat Analysis*
