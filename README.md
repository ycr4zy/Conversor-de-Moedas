# Conversor-de-Moedas
Conversor de Moedas feito com Python (Tkinker)

import tkinter as tk
from tkinter import messagebox
from tkinter import ttk

# Lista fixa das moedas mais comuns
moedas = [
    "USD", "BRL", "EUR", "GBP", "JPY", "ARS", "CAD", "AUD", "CHF", "CNY", "INR", "MXN", "RUB", "ZAR", "TRY", "KRW"
]


taxas = {
    ("USD", "BRL"): 5.2, ("BRL", "USD"): 0.19,
    ("USD", "EUR"): 0.92, ("EUR", "USD"): 1.09,
    ("USD", "GBP"): 0.78, ("GBP", "USD"): 1.28,
    ("USD", "JPY"): 156.0, ("JPY", "USD"): 0.0064,
    ("USD", "ARS"): 900.0, ("ARS", "USD"): 0.0011,
    ("USD", "CAD"): 1.36, ("CAD", "USD"): 0.74,
    ("USD", "AUD"): 1.52, ("AUD", "USD"): 0.66,
    ("USD", "CHF"): 0.89, ("CHF", "USD"): 1.12,
    ("USD", "CNY"): 7.25, ("CNY", "USD"): 0.14,
    ("USD", "INR"): 83.0, ("INR", "USD"): 0.012,
    ("USD", "MXN"): 17.0, ("MXN", "USD"): 0.059,
    ("USD", "RUB"): 90.0, ("RUB", "USD"): 0.011,
    ("USD", "ZAR"): 18.0, ("ZAR", "USD"): 0.055,
    ("USD", "TRY"): 32.0, ("TRY", "USD"): 0.031,
    ("USD", "KRW"): 1320.0, ("KRW", "USD"): 0.00076,
    
}

janela = tk.Tk()
janela.title("Conversor de Moedas")
janela.geometry("400x400")

combo_origem = ttk.Combobox(janela, values=moedas, state="readonly")
combo_origem.set("USD")
combo_origem.pack(pady=5)

combo_destino = ttk.Combobox(janela, values=moedas, state="readonly")
combo_destino.set("BRL")
combo_destino.pack(pady=5)

entrada_valor = tk.Entry(janela)
entrada_valor.pack(pady=5)

label_resultado = tk.Label(janela, text="", font=("Arial", 15))
label_resultado.pack(pady=10)

def converter_moeda():
    try:
        valor_float = float(entrada_valor.get().replace(",", "."))
        if valor_float <= 0:
            label_resultado.config(text="Digite um valor maior que zero.")
            return
    except:
        label_resultado.config(text="Digite um número válido.")
        return

    moeda_origem = combo_origem.get()
    moeda_destino = combo_destino.get()

    if moeda_origem == moeda_destino:
        convertido = valor_float
    else:
        par = (moeda_origem, moeda_destino)
        if par in taxas:
            convertido = valor_float * taxas[par]
        else:
            label_resultado.config(text="Conversão não disponível para essas moedas.")
            return

    label_resultado.config(
        text=f"{valor_float:.2f} {moeda_origem} = {convertido:.2f} {moeda_destino}"
    )
    adicionar_ao_historico(label_resultado.cget('text'))

def inverter_moedas():
    origem = combo_origem.get()
    destino = combo_destino.get()
    combo_origem.set(destino)
    combo_destino.set(origem)

botao_converter = tk.Button(janela, text="Converter", command=converter_moeda)
botao_converter.pack(pady=10)

botao_inverter = tk.Button(janela, text="Inverter moedas", command=inverter_moedas)
botao_inverter.pack(pady=10)

historico = tk.Listbox(janela, height=5)
historico.pack(pady=5)

def adicionar_ao_historico(texto):
    historico.insert(0, texto)
    if historico.size() > 5:
        historico.delete(5)

def ativar_modo_escuro():
    janela.configure(bg="#2e2e2e")
    label_resultado.configure(bg="#2e2e2e", fg="white")
    historico.configure(bg="#3e3e3e", fg="white")
    entrada_valor.configure(bg="#3e3e3e", fg="white")
    estilo = ttk.Style()
    estilo.theme_use('clam')
    estilo.configure("TCombobox", fieldbackground="#3e3e3e", background="#3e3e3e", foreground="white")

botao_escuro = tk.Button(janela, text="Modo Escuro", command=ativar_modo_escuro)
botao_escuro.pack(pady=10)

def ativar_modo_claro():
    janela.configure(bg="SystemButtonFace")
    label_resultado.configure(bg="SystemButtonFace", fg="black")
    historico.configure(bg="white", fg="black")
    entrada_valor.configure(bg="white", fg="black")
    estilo = ttk.Style()
    estilo.theme_use('default')
    estilo.configure("TCombobox", fieldbackground="white",foreground="black")

botao_claro = tk.Button(janela, text="Modo Claro", command=ativar_modo_claro)
botao_claro.pack(pady=10)


janela.mainloop()
