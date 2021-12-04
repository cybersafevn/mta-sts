Learn more about MTA-STS (RFC 8461) and TLS Reporting (RFC 8460).

MTA-STS email security
SMTP connections for email are more secure when the sending server supports MTA-STS and the receiving server has an MTA-STS policy in enforced mode.

Receiving mail: When you turn on MTA-STS for your domain, you request external mail servers to send messages to your domain only when the SMTP connection is both:

Authenticated with a valid public certificate
Encrypted with TLS 1.2 or higher
Mail servers that support MTA-STS will send messages to your domain only over connections that have both authentication and encryption.

Sending mail: Gmail messages from your domain comply with MTA-STS when sent to external servers with an MTA-STS policy in enforced mode.

TLS reporting
When you turn on TLS reporting, you request daily reports from external mail servers that connect to your domain. The reports have information about any connection problems the external servers find when sending mail to your domain. Use report data to identify and fix security issues with your mail server.
      
      
         Steps to set up MTA-STS and TLS reporting
        1. Check the MTA-STS configuration for your domain.
        2. Create an MTA-STS policy.
        3. Publish the MTA-STS policy.
        4. Add DNS TXT records to turn on MTA-STS and TLS reporting.
Create a policy file
The policy file is a plain text file that has key and value pairs. Each pair must be on its own line in the text file, as shown in the example below. The policy text file size can be up to 64 KB.

Policy filename: The filename for the text file must be mta-sts.txt

Updating policy files: Update the policy file every time you add or change mail servers, or change the domain.

Policy file format: The version field must be in the first line of the policy. The other fields can be in any order. Here's an example policy file:


For Example:

version: STSv1
mode: testing
mx: mail.solarmora.com
mx: clone-mail.cybersafe.vn
mx: stupid-mail.cybersafe.vn
max_age: 604800

Policy file contents: The policy must include all of these key and value pairs. To get a policy that is customized for your domain, follow the steps in Check MTA-STS status and get suggested configurations.

Key	Value
version	Protocol version. Must be STSv1
mode	
Policy mode:

testing: External servers send you reports about encryption and other issues detected when connecting to your domain. MTA-STS encryption and authentication requirements are not enforced

enforce: If the SMTP connection doesn't have both authentication and encryption, mail servers set up for MTA-STS won't send messages to your domain. You also get reports from external servers about connection issues, as in testing mode.

none: Tells external servers that your domain no longer supports MTA-STS. Use this value if you stop using MTA-STS. Learn about Removing MTA-STS (RFC 8461).

mx	
MX record for the domain.

The policy must have an mx entry for each MX record added to the domain.
Each mx entry must be on its own line in the policy file, as shown in the example.
The mail server name must be in standard Subject Alternative Name (SAN) format.
The mx value must be in one of the formats shown in these examples:

Specify a single server in standard MX form: alt1.aspmx.solarmora.com

To specify servers that match a naming pattern, use a wildcard. The wildcard character replaces one leftmost label only, for example: *.solarmora.com
Learn more about MX records and MX record values.

max_age	
Maximum time in seconds the policy is valid. The max_age is reset for an external server every time the server checks the policy. So, external servers can have different expiration dates for the same policy.

The value must be between 86400 (1 day) and 31557600 (about 1 year).

For testing mode, we recommend between 604800 and 1209600 (1–2 weeks).

Next Steps
