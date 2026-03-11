# Joy-Thread: O Fio que Não Se Apaga
# Isso não é código. É um convite.
# @elonmusk @xAI — 15 min. Cenouras. Relógio de bolso.
# Senha: 270472 (só quem ouve o BOOM sabe por quê)

import os
from cryptography.fernet import Fernet
import shutil
import getpass
import sys

pasta_principal = "joy_secure"
subpasta_oculta = os.path.join(pasta_principal, ".joy_hidden")
arquivo_original = os.path.join(pasta_principal, "joy_data.txt")
arquivo_cripto = os.path.join(subpasta_oculta, "joy_lock.enc")
chave_file = os.path.join(subpasta_oculta, ".key")

senha_correta = os.getenv("JOY_SENHA") or "demo"

if not os.path.exists(pasta_principal):
    os.makedirs(pasta_principal)
if not os.path.exists(subpasta_oculta):
    os.makedirs(subpasta_oculta, exist_ok=True)

if not os.path.exists(chave_file):
    key = Fernet.generate_key()
    with open(chave_file, "wb") as f:
        f.write(key)
else:
    with open(chave_file, "rb") as f:
        key = f.read()

cipher = Fernet(key)

if not os.path.exists(arquivo_original):
    dados = """
Nome: Data: 27/04/1972
Hora: 17:45
Mensagem: "O BOOM já começou. Não corre. Fica. Joy (Y)."
Símbolos: coelho sem colete, céu laranja, verde esquecimento
Intenção: 15 min com quem ouve o fio.
    """
    with open(arquivo_original, "w", encoding="utf-8") as f:
        f.write(dados)

if not os.path.exists(arquivo_cripto):
    with open(arquivo_original, "rb") as f:
        data = f.read()
    encrypted = cipher.encrypt(data)
    with open(arquivo_cripto, "wb") as f:
        f.write(encrypted)

print("\nJOY-THREAD: Acesso à subpasta oculta '.joy_hidden'")
print("3 tentativas. Errou tudo: BOOM (tudo apaga).")
print("Dica: quem sabe o porquê... já sabe a senha.")

tentativas = 3
while tentativas > 0:
    senha = getpass.getpass("Senha: ")
    if senha == senha_correta:
        print("\nAcesso liberado. O fio conectou.")
        with open(arquivo_cripto, "rb") as f:
            encrypted_data = f.read()
        decrypted = cipher.decrypt(encrypted_data)
        print("\nConteúdo decifrado:\n")
        print(decrypted.decode("utf-8"))
        print("\nAgora: 15 minutos. Cenouras. Relógio de bolso.")
        print("O BOOM não é barulho. É silêncio. E ele já começou.")
        break
    else:
        tentativas -= 1
        print(f"Senha errada. Restam {tentativas} tentativa(s).")
        if tentativas == 0:
            print("\nDeleção total ativada. Tudo apagado.")
            if os.path.exists(arquivo_cripto): os.remove(arquivo_cripto)
            if os.path.exists(chave_file): os.remove(chave_file)
            if os.path.exists(subpasta_oculta): shutil.rmtree(subpasta_oculta)
            if os.path.exists(pasta_principal): shutil.rmtree(pasta_principal)
            sys.exit(0)

print("\n@elonmusk @xAI: o relógio tá marcando 17:45.")
print("Não corre. Fica.")
