Task 1 — Classify and Handle PII Fields
1. Field Name: full_name

Classification: Direct PII

Action: Drop

Justification: This identifies the individual directly and is not required for statistical health research.

2. Field Name: email

Classification: Direct PII

Action: Drop

Justification: Contact information is highly sensitive and poses a high identity theft risk if leaked.

3. Field Name: date_of_birth

Classification: Indirect PII

Action: Mask

Justification: Specific dates can help re-identify individuals; it should be converted into age ranges or birth years instead.

4. Field Name: zip_code

Classification: Indirect PII

Action: Mask

Justification: Precise location data can lead to identification; truncating to the first 3 digits provides general region data while protecting privacy.

5. Field Name: job_title

Classification: Indirect PII

Action: Pseudonymize

Justification: Rare or high-profile job titles can identify people; these should be replaced with broad categories like "Healthcare Worker."

6. Field Name: diagnosis_notes

Classification: Sensitive Personal Info

Action: Pseudonymize

Justification: These notes contain unique health details and must be scrubbed of any specific names or dates within the text.



import time
import requests

records = []
for page in range(1, 101):
    try:
        response = requests.get(API_URL, params={"page": page, "key": API_KEY})
        
        # Check for successful response
        if response.status_code == 200:
            data = response.json()
            records.extend(data.get("results", []))
        
        # Ethical Compliance: Add a delay to respect server limits
        time.sleep(1) 
        
    except Exception as e:
        print(f"Error on page {page}: {e}")
        break

# Avoid "permanent" storage of sensitive PII unless strictly legally required
# save_to_database(records)
