# **AWS : Building Serverless Application**
### **Services used** :
    Amazon Lex, Amazon S3, 

### **Dataflow Diagram** :
![AWS_CopyData_Between_S3](https://user-images.githubusercontent.com/96478746/169760292-90e69a83-95dd-43b2-b80c-4b285d8aea3b.jpg)

### **Creating a Lex Bot**

Our first goal is to create a simple chatbot in the LEX console that can gather basic information from a user, which we call "eliciting slots".

**Steps for creating a LEX bot**

    1. Go to Amazon Lex
    2. Make sure you are in the N. Virginia region at the top right
    3. Under Create Your Own choose Custom Bot
    4. Name the Bot: WeatherCatBot
    5. For Output voice, Leave it as: None (This is only a text based application)
    6. Change the Session timeout to 1 minute
    7. Leave the IAM role as ANSServiceRoleForLexBots
    8. For COPPA choose No
