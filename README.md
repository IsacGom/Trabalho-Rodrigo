import sqlite3
import tkinter as tk
from tkinter import messagebox, simpledialog

def criar_tabela_clientes():
    conn = sqlite3.connect('academia.db')
    cursor = conn.cursor()
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS clientes (
            id INTEGER PRIMARY KEY,
            nome TEXT,
            idade INTEGER,
            sexo TEXT,
            endereco TEXT,
            telefone TEXT
        )
    ''')
    conn.commit()
    conn.close()

def adicionar_cliente():
    nome = nome_entry.get()
    idade = idade_entry.get()
    sexo = sexo_entry.get()
    endereco = endereco_entry.get()
    telefone = telefone_entry.get()

    conn = sqlite3.connect('academia.db')
    cursor = conn.cursor()
    cursor.execute('INSERT INTO clientes (nome, idade, sexo, endereco, telefone) VALUES (?, ?, ?, ?, ?)', (nome, idade, sexo, endereco, telefone))
    conn.commit()
    conn.close()

    messagebox.showinfo("Sucesso", "Cliente adicionado com sucesso!")

def listar_clientes():
    conn = sqlite3.connect('academia.db')
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM clientes')
    clientes = cursor.fetchall()
    conn.close()

    if clientes:
        clientes_str = "Lista de Clientes:\n\n"
        for cliente in clientes:
            clientes_str += f"ID: {cliente[0]}\nNome: {cliente[1]}\nIdade: {cliente[2]}\nSexo: {cliente[3]}\nEndereço: {cliente[4]}\nTelefone: {cliente[5]}\n\n"
        messagebox.showinfo("Clientes Cadastrados", clientes_str)
    else:
        messagebox.showinfo("Nenhum cliente", "Nenhum cliente cadastrado.")

def excluir_cliente():
    cliente_id = int(excluir_cliente_id_entry.get())

    conn = sqlite3.connect('academia.db')
    cursor = conn.cursor()
    cursor.execute('DELETE FROM clientes WHERE id=?', (cliente_id,))
    conn.commit()
    conn.close()

    messagebox.showinfo("Sucesso", "Cliente excluído!")

def atualizar_cliente():
    cliente_id = int(excluir_cliente_id_entry.get())
    conn = sqlite3.connect('academia.db')
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM clientes WHERE id=?', (cliente_id,))
    cliente = cursor.fetchone()
    conn.close()

    if cliente:
        # Pedir novos detalhes do cliente
        novo_nome = simpledialog.askstring("Atualizar Cliente", "Novo Nome:", initialvalue=cliente[1])
        nova_idade = simpledialog.askinteger("Atualizar Cliente", "Nova Idade:", initialvalue=cliente[2])
        novo_sexo = simpledialog.askstring("Atualizar Cliente", "Novo Sexo:", initialvalue=cliente[3])
        novo_endereco = simpledialog.askstring("Atualizar Cliente", "Novo Endereço:", initialvalue=cliente[4])
        novo_telefone = simpledialog.askstring("Atualizar Cliente", "Novo Telefone:", initialvalue=cliente[5])

        if novo_nome is not None:
            conn = sqlite3.connect('academia.db')
            cursor = conn.cursor()
            cursor.execute('''
                UPDATE clientes
                SET nome=?, idade=?, sexo=?, endereco=?, telefone=?
                WHERE id=?
            ''', (novo_nome, nova_idade, novo_sexo, novo_endereco, novo_telefone, cliente_id))
            conn.commit()
            conn.close()
            messagebox.showinfo("Sucesso", "Detalhes do cliente atualizados!")
    else:
        messagebox.showinfo("Erro", "Cliente não encontrado.")

def main():
    criar_tabela_clientes()

    root = tk.Tk()
    root.title("Sistema de Gestão de Academias")

    # Configuração de fonte e estilo
    font = ("Arial", 12)
    bg_color = "#f0f0f0"  # Cor de fundo
    label_color = "#333"  # Cor dos rótulos

    root.configure(bg=bg_color)

    tk.Label(root, text="Nome:", bg=bg_color, fg=label_color, font=font).grid(row=0, column=0)
    tk.Label(root, text="Idade:", bg=bg_color, fg=label_color, font=font).grid(row=1, column=0)
    tk.Label(root, text="Sexo:", bg=bg_color, fg=label_color, font=font).grid(row=2, column=0)
    tk.Label(root, text="Endereço:", bg=bg_color, fg=label_color, font=font).grid(row=3, column=0)
    tk.Label(root, text="Telefone:", bg=bg_color, fg=label_color, font=font).grid(row=4, column=0)

    global nome_entry, idade_entry, sexo_entry, endereco_entry, telefone_entry
    nome_entry = tk.Entry(root, font=font)
    idade_entry = tk.Entry(root, font=font)
    sexo_entry = tk.Entry(root, font=font)
    endereco_entry = tk.Entry(root, font=font)
    telefone_entry = tk.Entry(root, font=font)

    nome_entry.grid(row=0, column=1)
    idade_entry.grid(row=1, column=1)
    sexo_entry.grid(row=2, column=1)
    endereco_entry.grid(row=3, column=1)
    telefone_entry.grid(row=4, column=1)

    adicionar_cliente_button = tk.Button(root, text="Adicionar Cliente", command=adicionar_cliente, font=font)
    listar_clientes_button = tk.Button(root, text="Listar Clientes", command=listar_clientes, font=font)
    excluir_cliente_button = tk.Button(root, text="Excluir Cliente", command=excluir_cliente, font=font)
    atualizar_cliente_button = tk.Button(root, text="Atualizar Cliente", command=atualizar_cliente, font=font)

    adicionar_cliente_button.grid(row=5, column=0, columnspan=2)
    listar_clientes_button.grid(row=6, column=0, columnspan=2)
    excluir_cliente_button.grid(row=7, column=0, columnspan=2)
    atualizar_cliente_button.grid(row=8, column=0, columnspan=2)

    tk.Label(root, text="ID do Cliente:", bg=bg_color, fg=label_color, font=font).grid(row=9, column=0)
    global excluir_cliente_id_entry
    excluir_cliente_id_entry = tk.Entry(root, font=font)
    excluir_cliente_id_entry.grid(row=9, column=1)

    root.mainloop()

main()

#Treinador

import sqlite3
import tkinter as tk
from tkinter import messagebox, simpledialog

def criar_tabela_treinadores():
    conn = sqlite3.connect('academia.db')
    cursor = conn.cursor()
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS treinadores (
            id INTEGER PRIMARY KEY,
            nome TEXT,
            especialidade TEXT,
            salario REAL,
            telefone TEXT
        )
    ''')
    conn.commit()
    conn.close()

def adicionar_treinador():
    nome = simpledialog.askstring("Adicionar Treinador", "Nome:")
    especialidade = simpledialog.askstring("Adicionar Treinador", "Especialidade:")
    salario = simpledialog.askfloat("Adicionar Treinador", "Salário:")
    telefone = simpledialog.askstring("Adicionar Treinador", "Telefone:")

    conn = sqlite3.connect('academia.db')
    cursor = conn.cursor()
    cursor.execute('INSERT INTO treinadores (nome, especialidade, salario, telefone) VALUES (?, ?, ?, ?)', (nome, especialidade, salario, telefone))
    conn.commit()
    conn.close()

    messagebox.showinfo("Sucesso", "Treinador adicionado com sucesso!")

def listar_treinadores():
    conn = sqlite3.connect('academia.db')
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM treinadores')
    treinadores = cursor.fetchall()
    conn.close()

    if treinadores:
        treinadores_str = "Lista de Treinadores:\n\n"
        for treinador in treinadores:
            treinadores_str += f"ID: {treinador[0]}\nNome: {treinador[1]}\nEspecialidade: {treinador[2]}\nSalário: {treinador[3]}\nTelefone: {treinador[4]}\n\n"
        messagebox.showinfo("Treinadores Cadastrados", treinadores_str)
    else:
        messagebox.showinfo("Nenhum treinador", "Nenhum treinador cadastrado.")

def excluir_treinador():
    treinador_id = simpledialog.askinteger("Excluir Treinador", "ID do Treinador:")

    conn = sqlite3.connect('academia.db')
    cursor = conn.cursor()
    cursor.execute('DELETE FROM treinadores WHERE id=?', (treinador_id,))
    conn.commit()
    conn.close()

    messagebox.showinfo("Sucesso", "Treinador excluído!")

def atualizar_treinador():
    treinador_id = simpledialog.askinteger("Atualizar Treinador", "ID do Treinador:")
    conn = sqlite3.connect('academia.db')
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM treinadores WHERE id=?', (treinador_id,))
    treinador = cursor.fetchone()
    conn.close()

    if treinador:
        novo_nome = simpledialog.askstring("Atualizar Treinador", "Novo Nome:", initialvalue=treinador[1])
        nova_especialidade = simpledialog.askstring("Atualizar Treinador", "Nova Especialidade:", initialvalue=treinador[2])
        novo_salario = simpledialog.askfloat("Atualizar Treinador", "Novo Salário:", initialvalue=treinador[3])
        novo_telefone = simpledialog.askstring("Atualizar Treinador", "Novo Telefone:", initialvalue=treinador[4])

        if novo_nome is not None:
            conn = sqlite3.connect('academia.db')
            cursor = conn.cursor()
            cursor.execute('''
                UPDATE treinadores
                SET nome=?, especialidade=?, salario=?, telefone=?
                WHERE id=?
            ''', (novo_nome, nova_especialidade, novo_salario, novo_telefone, treinador_id))
            conn.commit()
            conn.close()
            messagebox.showinfo("Sucesso", "Detalhes do treinador atualizados!")
    else:
        messagebox.showinfo("Erro", "Treinador não encontrado.")

def main():
    criar_tabela_treinadores()

    root = tk.Tk()
    root.title("Sistema de Gestão de Treinadores")

    font = ("Arial", 12)
    bg_color = "#f0f0f0"
    label_color = "#333"

    root.configure(bg=bg_color)

    adicionar_treinador_button = tk.Button(root, text="Adicionar Treinador", command=adicionar_treinador, font=font)
    adicionar_treinador_button.pack()

    listar_treinadores_button = tk.Button(root, text="Listar Treinadores", command=listar_treinadores, font=font)
    listar_treinadores_button.pack()

    excluir_treinador_button = tk.Button(root, text="Excluir Treinador", command=excluir_treinador, font=font)
    excluir_treinador_button.pack()

    atualizar_treinador_button = tk.Button(root, text="Atualizar Treinador", command=atualizar_treinador, font=font)
    atualizar_treinador_button.pack()

    root.mainloop()

main()
