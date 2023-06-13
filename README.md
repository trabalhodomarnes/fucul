# fucul
trabalhos da faculdade
import sqlite3

# Conectar ao banco de dados do sq
conexao = sqlite3.connect('horas_extras.db')
cursor = conexao.cursor()

# cria-se a tabela para armazenar as horas extras, caso nao tenha(usei o sqlite,tem que baixar pra ver a tabela)
cursor.execute('''CREATE TABLE IF NOT EXISTS horas_extras (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    funcionario TEXT,
                    data TEXT,
                    horas INTEGER
                )''')

def registrar_horas_extra(funcionario, data, horas):
    # aqui é inserido o registro de horas extras na tabela
    cursor.execute("INSERT INTO horas_extras (funcionario, data, horas) VALUES (?, ?, ?)", (funcionario, data, horas))
    conexao.commit()
    print("Horas extras registradas com sucesso!")

def calcular_pagamento_adicional():
    # calculo o pagamento adicional de acordo com as horas extras registradas
    cursor.execute("SELECT funcionario, SUM(horas) FROM horas_extras GROUP BY funcionario")
    resultados = cursor.fetchall()
    
    for resultado in resultados:
        funcionario = resultado[0]
        total_horas = resultado[1]
        pagamento_adicional = total_horas * 1.5  # Supondo que o pagamento adicional seja 1,5 vezes o valor da hora normal
        print(f"Funcionário: {funcionario} | Total de Horas Extras: {total_horas} | Pagamento Adicional: R${pagamento_adicional:.2f}")

# aqui eu registro a hora extra para funcionários
registrar_horas_extra("João", "2023-05-15", 2)
registrar_horas_extra("Maria", "2023-05-15", 3)
registrar_horas_extra("Pedro", "2023-05-16", 1)
registrar_horas_extra("João", "2023-05-16", 2)

# calcular pagamento adicional de acordo com as horas
calcular_pagamento_adicional()

# fecho conexão com o banco de dados
conexao.close()
