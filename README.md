import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.base import MIMEBase
from email import encoders

def enviar_email():
    fromaddr = "seuemail@gmail.com"
    toaddr = "destinatario@gmail.com"
    
    msg = MIMEMultipart()
    msg['From'] = fromaddr
    msg['To'] = toaddr
    msg['Subject'] = "Assunto do E-mail"
    
    body = "Corpo do E-mail"
    msg.attach(MIMEText(body, 'plain'))
    
    filename = "anexo.txt"
    attachment = open("caminho/para/o/anexo.txt", "rb")
    
    part = MIMEBase('application', 'octet-stream')
    part.set_payload((attachment).read())
    encoders.encode_base64(part)
    part.add_header('Content-Disposition', "attachment; filename= %s" % filename)
    
    msg.attach(part)
    
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.starttls()
    server.login(fromaddr, "sua_senha")
    text = msg.as_string()
    server.sendmail(fromaddr, toaddr, text)
    server.quit()

enviar_email()
