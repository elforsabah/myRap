import base64

# Specify the path to your PDF file
pdf_path = "example.pdf"

# Read the PDF file in binary mode
with open(pdf_path, "rb") as pdf_file:
    # Encode the PDF file to Base64
    base64_string = base64.b64encode(pdf_file.read()).decode("utf-8")

# Print or use the Base64 string
print(base64_string)
