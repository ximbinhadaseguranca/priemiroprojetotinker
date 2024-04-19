# priemiroprojetotinker
maluqieci isso


eu tentei algo osuado porfessor pede uma coisa a gente tenta outra, fiz ngc bem basico pois n lembro direito como faz...



import tkinter as tk

class Usuario:
    def __init__(self, nome, senha, privilegio):
        self.nome = nome
        self.senha = senha
        self.privilegio = privilegio

class SistemaCadastro:
    def __init__(self):
        self.usuarios = []

    def cadastrar_usuario(self, nome, senha, privilegio):
        novo_usuario = Usuario(nome, senha, privilegio)
        self.usuarios.append(novo_usuario)
        print("Usuário cadastrado com sucesso!")

    def verificar_privilegio(self, nome_usuario):
        for usuario in self.usuarios:
            if usuario.nome == nome_usuario:
                return usuario.privilegio
        return None

    def verificar_senha(self, nome_usuario, senha):
        for usuario in self.usuarios:
            if usuario.nome == nome_usuario:
                if usuario.senha == senha:
                    return True
                else:
                    return False
        return False

    def alterar_senha(self, nome_usuario, senha_atual, nova_senha):
        for usuario in self.usuarios:
            if usuario.nome == nome_usuario:
                if usuario.senha == senha_atual:
                    usuario.senha = nova_senha
                    print("Senha alterada com sucesso para o usuário", nome_usuario)
                    return True
                else:
                    print("Senha atual incorreta.")
                    return False
        print("Usuário não encontrado.")
        return False

class TelaLogin:
    def __init__(self, master, sistema):
        self.master = master
        self.sistema = sistema
        master.title("Login")

        self.label_usuario = tk.Label(master, text="Usuário:")
        self.label_senha = tk.Label(master, text="Senha:")

        self.entry_usuario = tk.Entry(master)
        self.entry_senha = tk.Entry(master, show="*")

        self.label_usuario.grid(row=0, sticky=tk.E)
        self.label_senha.grid(row=1, sticky=tk.E)
        self.entry_usuario.grid(row=0, column=1)
        self.entry_senha.grid(row=1, column=1)

        self.login_button = tk.Button(master, text="Login", command=self.fazer_login)
        self.login_button.grid(columnspan=2)

        self.label_mensagem = tk.Label(master, text="")
        self.label_mensagem.grid(columnspan=2)

    def fazer_login(self):
        nome_usuario = self.entry_usuario.get()
        senha = self.entry_senha.get()
        privilegio = self.sistema.verificar_privilegio(nome_usuario)
        if privilegio:
            if self.sistema.verificar_senha(nome_usuario, senha):
                if privilegio == "SUPERUSER":
                    self.master.destroy()  # Fecha a janela de login
                    root2 = tk.Tk()
                    app2 = TelaAdmin(root2, self.sistema)  # Abre a tela de administração
                    root2.mainloop()
                elif privilegio == "NORMAL":
                    self.master.destroy()  # Fecha a janela de login
                    root3 = tk.Tk()
                    app3 = TelaUsuario(root3)  # Abre a tela do usuário comum
                    root3.mainloop()
                else:
                    print("Login bem sucedido!")
            else:
                self.label_mensagem.config(text="Senha incorreta")
        else:
            print("Usuário não encontrado.")

class TelaAdmin:
    def __init__(self, master, sistema):
        self.master = master
        self.sistema = sistema
        master.title("Admin")

        self.label_novo_usuario = tk.Label(master, text="Novo Usuário:")
        self.label_nova_senha = tk.Label(master, text="Nova Senha:")

        self.entry_novo_usuario = tk.Entry(master)
        self.entry_nova_senha = tk.Entry(master, show="*")

        self.label_novo_usuario.grid(row=0, sticky=tk.E)
        self.label_nova_senha.grid(row=1, sticky=tk.E)
        self.entry_novo_usuario.grid(row=0, column=1)
        self.entry_nova_senha.grid(row=1, column=1)

        self.cadastrar_button = tk.Button(master, text="Cadastrar Usuário", command=self.cadastrar_usuario)
        self.cadastrar_button.grid(columnspan=2)

        self.label_mensagem = tk.Label(master, text="")
        self.label_mensagem.grid(row=2, columnspan=2)

        # Botão para voltar ao menu principal
        self.voltar_button = tk.Button(master, text="Voltar", command=self.voltar_menu)
        self.voltar_button.grid(row=3, columnspan=2)

    def cadastrar_usuario(self):
        nome_usuario = self.entry_novo_usuario.get()
        senha = self.entry_nova_senha.get()
        privilegio = "NORMAL"  # Definimos como NORMAL por padrão, pode ser ajustado conforme necessário
        self.sistema.cadastrar_usuario(nome_usuario, senha, privilegio)
        self.label_mensagem.config(text="Usuário cadastrado com sucesso.")

    def voltar_menu(self):
        self.master.destroy()  # Fecha a janela de administração
        root = tk.Tk()
        app = TelaLogin(root, self.sistema)  # Volta para a tela de login
        root.mainloop()

class TelaUsuario:
    def __init__(self, master):
        self.master = master
        master.title("Usuário Comum")

        # Exibir texto simples
        self.texto_label = tk.Label(master, text="Alberico não dá CP pvr")
        self.texto_label.pack()

        # Botão para voltar ao menu principal
        self.voltar_button = tk.Button(master, text="Voltar", command=self.voltar_menu)
        self.voltar_button.pack()

    def voltar_menu(self):
        self.master.destroy()  # Fecha a janela do usuário comum
        root = tk.Tk()
        app = TelaLogin(root, sistema)  # Volta para a tela de login
        root.mainloop()

# Criando instância do sistema de cadastro
sistema = SistemaCadastro()
sistema.cadastrar_usuario("admin", "admin123", "SUPERUSER")
sistema.cadastrar_usuario("usuario1", "senha123", "NORMAL")
sistema.cadastrar_usuario("usuario2", "senha456", "NORMAL")

root = tk.Tk()
app = TelaLogin(root, sistema)  # Inicia na tela de login
root.mainloop()
