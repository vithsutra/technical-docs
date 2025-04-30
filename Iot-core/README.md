Here is your complete guide in `.md` (Markdown) format:

---

# 🚀 AWS IoT MQTT → Lambda → MongoDB Integration Guide

This guide walks you step-by-step through the process of:

✅ Connecting a **local MQTT client (mosquitto_pub)**  
✅ Publishing to **AWS IoT Core**  
✅ Triggering a **Lambda function**  
✅ Storing data in **MongoDB**

---

## ✅ STEP 1: Generate AWS IoT Certificates

1. Go to **AWS IoT Console → Secure → Certificates**  
2. Click **Create certificate** under the **1-Click Create** tab  
3. Download the following files:
   - `certificate.pem.crt`
   - `private.pem.key`
   - `AmazonRootCA1.pem` ([Download here](https://www.amazontrust.com/repository/AmazonRootCA1.pem))
4. Click **Activate**
5. Click **Attach a policy**

---

## ✅ STEP 2: Create and Attach a Policy

**Sample Policy (for dev/test):**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Connect",
        "iot:Publish"
      ],
      "Resource": "*"
    }
  ]
}
```

1. Go to **Secure → Policies → Create**
2. Name it (e.g., `MQTTFullAccess`)
3. Paste the JSON policy above
4. Create and **attach it** to the certificate

---

## ✅ STEP 3: Create a Thing and Attach Certificate

1. Go to **Manage → Things → Create a Thing**
2. Choose **Single Thing**
3. Name it (e.g., `device1`)
4. Skip Device Shadow (unless needed)
5. Attach your **existing certificate**
6. Finish setup

---

## ✅ STEP 4: Create IoT Rule to Trigger Lambda

1. Go to **Act → Create a Rule**
2. Name: `ForwardToLambda`
3. Rule SQL:

   ```sql
   SELECT * FROM 'device/device1/data'
   ```

4. Action: **Add action → Invoke Lambda function**
5. Select your Lambda function
6. Grant necessary permissions when prompted
7. Create the rule

---

## ✅ STEP 5: Test `mosquitto_pub` Locally

Ensure you're in the folder containing:

- `certificate.pem.crt`
- `private.pem.key`
- `AmazonRootCA1.pem`

Run this command (replace endpoint as needed):

```bash
mosquitto_pub \
  --cafile AmazonRootCA1.pem \
  --cert certificate.pem.crt \
  --key private.pem.key \
  -h a8jmdqpykxxu3-ats.iot.us-east-1.amazonaws.com \
  -p 8883 \
  -t device/device1/data \
  -m '{"temp": 25}' \
  --tls-version tlsv1.2 \
  --id testClient123 \
  -d
```

> ✅ Ensure:
> - The `-h` endpoint is your **AWS IoT endpoint**
> - The topic matches your **IoT rule**

---

## ✅ STEP 6: Confirm Lambda & MongoDB Insertion

- Go to **CloudWatch Logs** and check your Lambda log group for:
  - Invocation confirmation
  - Any error tracebacks
- Check **MongoDB** to ensure the data has been successfully inserted

---

## ✅ Bonus Debug Tips

If you get errors like:

```bash
Client null sending CONNECT
Error: The connection was lost.
```

Check the following:

- 🔁 Certificates don’t match (try re-creating them)
- 📜 Policy isn’t attached or lacks permissions
- 🌐 Wrong endpoint or MQTT topic
- 💥 Lambda function errors — check **CloudWatch logs**

---

Let me know if you'd like a `.zip` version of this guide or an example Lambda + MongoDB code!
