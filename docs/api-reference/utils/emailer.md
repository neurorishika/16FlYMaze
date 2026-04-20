# Emailer

**Module:** `sixteeny.utils.emailer`

Sends automated experiment progress notifications via the Gmail API.

---

## Class: `Emailer`

```python
Emailer(
    config: str = "sixteeny/utils/email_config.json",
    recipients: list[str] = None,
    printer: Printer = None
)
```

| Parameter | Type | Description |
|---|---|---|
| `config` | `str` | Path to the email configuration JSON file |
| `recipients` | `list[str]` | Override recipient email addresses (uses config file value if `None`) |
| `printer` | `Printer` | Optional `Printer` instance for logging |

### Configuration File Format

`email_config.json`:
```json
{
    "CLIENT_SECRET_FILE": "sixteeny/utils/client_secret.json",
    "SENDER_ACCOUNT": "sender@gmail.com",
    "RECIPIENT_ACCOUNTS": ["recipient1@example.com", "recipient2@example.com"]
}
```

### Methods

```python
def get_credentials(self) -> oauth2client.Credentials
```
Retrieve (or refresh) OAuth2 credentials for the Gmail API. Opens a browser authentication flow on first use.

```python
def SendMessage(
    self,
    subject: str,
    body_text: str,
    body_html: str
) -> dict
```
Send an email with the given subject, plain-text body, and HTML body.

| Parameter | Type | Description |
|---|---|---|
| `subject` | `str` | Email subject line |
| `body_text` | `str` | Plain-text email body |
| `body_html` | `str` | HTML email body |

Returns the Gmail API send response dict on success, `None` on error.

---

## Setup

1. Create OAuth2 credentials in [Google API Console](https://console.developers.google.com/apis/credentials)
2. Download as `client_secret.json` and place in `sixteeny/utils/`
3. Run the authorization script once to generate the token:
   ```bat
   test_and_authorize_email.bat
   ```

---

## Usage Example

```python
from sixteeny.utils.emailer import Emailer

emailer = Emailer(
    config="sixteeny/utils/email_config.json",
    recipients=["pi@example.com"]
)

emailer.SendMessage(
    subject="Experiment Started",
    body_text="Experiment 'my_exp' has started with 16 arenas.",
    body_html="<b>Experiment</b> my_exp has started."
)
```

---

## Dependencies

| Package | Purpose |
|---|---|
| `google-api-python-client` | Gmail REST API |
| `oauth2client` | OAuth2 authentication |
| `httplib2` | HTTP transport |
