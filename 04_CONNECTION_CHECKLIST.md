# Connection checklist — do one at a time

> Never paste a password, API key, or token into a chat or an untrusted document.

## 1. Hosted n8n

- Create a hosted n8n workspace.
- Do not activate any workflow yet.
- Create a project named `Sunnyworks Outbound — Draft Only`.

## 2. Apollo — lead source

Apollo's official API uses an API key. Create a **scoped** key, not a master key.

Where to create it: Apollo → **Settings → Integrations → API Keys → Create new key**.

Request only the scopes needed for the first draft workflow:

- `mixed_people_api_search` — finds new prospects
- `people_match` — only if you later choose to enrich a business email

Apollo uses the key in the `x-api-key` header. API access depends on the Apollo plan and the selected key scopes.

## 3. Notion — private lead queue

Create an internal Notion connection limited to the one **Sunnyworks Outbound Queue** database.

Where to create it: Notion Developer portal → **Build → Internal connections → Create a new connection**. Retrieve the token from its **Configuration** tab, then grant only the lead database in **Content access**.

Give it only the capabilities needed to read and update this one database.

## 4. Gmail — sending inbox

In hosted n8n, create a Gmail credential and complete the Google sign-in/consent flow for the sending inbox.

Before sending to prospects, verify that the sending domain has SPF, DKIM, and DMARC configured. Start at no more than three emails per weekday.

## 5. OpenAI API — drafting only

Create a separate OpenAI API project/key for n8n rather than reusing a key elsewhere. The API account is separate from a ChatGPT subscription and may require its own billing setup.

Use the key only in n8n's OpenAI credential. Restrict the workflow to company-level public data. Do not put contact emails, client data, passwords, or contract information in model input.

## The order to use

1. Connect n8n
2. Create the empty Notion database
3. Connect Notion to n8n
4. Connect Apollo to n8n
5. Connect Gmail to n8n
6. Connect OpenAI to n8n
7. Run internal tests only

No workflow is activated until explicitly approved.
