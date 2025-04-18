import requests
from bs4 import BeautifulSoup
import pandas as pd

def scrape_data(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')

    data = []
    leads = soup.find_all('div', class_='lead-card')  # Update with real tag/class

    for lead in leads:
        name = lead.find('h2').text.strip()
        email = lead.find('a', class_='email').text.strip()
        company = lead.find('span', class_='company').text.strip()
        data.append({'Name': name, 'Email': email, 'Company': company})

    return pd.DataFrame(data)
import pandas as pd

def save_to_excel(dataframe, path='data/leads.xlsx'):
    dataframe.to_excel(path, index=False)
    print(f"[+] Data saved to {path}")
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

def send_email(to_email, subject, html_content):
    from_email = "your-email@example.com"
    password = "your-email-password"

    msg = MIMEMultipart("alternative")
    msg['Subject'] = subject
    msg['From'] = from_email
    msg['To'] = to_email

    msg.attach(MIMEText(html_content, "html"))

    with smtplib.SMTP_SSL("smtp.gmail.com", 465) as server:
        server.login(from_email, password)
        server.sendmail(from_email, to_email, msg.as_string())

    print(f"[+] Email sent to {to_email}")
    import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

def send_email(to_email, subject, html_content):
    from_email = "your-email@example.com"
    password = "your-email-password"

    msg = MIMEMultipart("alternative")
    msg['Subject'] = subject
    msg['From'] = from_email
    msg['To'] = to_email

    msg.attach(MIMEText(html_content, "html"))

    with smtplib.SMTP_SSL("smtp.gmail.com", 465) as server:
        server.login(from_email, password)
        server.sendmail(from_email, to_email, msg.as_string())

    print(f"[+] Email sent to {to_email}")
import pandas as pd
from email_sender import send_email

def run_email_campaign(excel_path='data/leads.xlsx', template_path='email_templates/email_template.html'):
    df = pd.read_excel(excel_path)

    with open(template_path, 'r') as f:
        html_template = f.read()

    for _, row in df.iterrows():
        personalized = html_template.replace('{name}', row['Name'])
        send_email(row['Email'], "Exciting Offer Just for You!", personalized)
from sklearn.ensemble import RandomForestClassifier
import pandas as pd

def train_model(data):
    X = data[['feature1', 'feature2']]  # Replace with actual features
    y = data['converted']  # Binary classification target
    model = RandomForestClassifier()
    model.fit(X, y)
    return model
from scripts.web_scraper import scrape_data
from scripts.save_to_excel import save_to_excel
from scripts.email_automation import run_email_campaign

def main():
    print("[*] Starting sales automation process...")

    url = "https://example.com/leads"  # Replace with actual source
    df = scrape_data(url)
    save_to_excel(df)

    run_email_campaign()

if __name__ == "__main__":
    main()
# Sales Process Automation using Python + AIML

This project automates the sales process including:
- Web scraping of leads
- Saving leads to Excel
- Sending personalized emails
- Basic lead scoring with ML

## Setup

```bash
pip install -r requirements.txt

