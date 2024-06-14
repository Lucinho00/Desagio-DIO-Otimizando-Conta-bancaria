# Desagio-DIO-Otimizando-Conta-bancaria

class Usuario:
    def __init__(self, nome, data_nascimento, cpf, endereco):
        self.nome = nome
        self.data_nascimento = data_nascimento
        self.cpf = cpf
        self.endereco = endereco


class ContaCorrente:
    agencia = '0001'  # Agência fixa para todas as contas
    proximo_numero_conta = 1  # Contador para gerar números de conta sequenciais

    def __init__(self, usuario):
        self.usuario = usuario
        self.numero_conta = ContaCorrente.proximo_numero_conta
        ContaCorrente.proximo_numero_conta += 1
        self.saldo = 0.0
        self.movimentacoes = []
        self.limite_saque = 1000.0  # Limite máximo de saque por operação
        self.limite_saques_diarios = 5  # Limite de saques diários
        self.saques_diarios = 0

    def depositar(self, saldo, valor, extrato=None):
        if valor > 0:
            saldo += valor
            if extrato is not None:
                extrato.append(f"Depósito: R$ {valor:.2f}")
            print(f"Depósito de R$ {valor:.2f} realizado com sucesso.")
        else:
            print("Valor de depósito inválido.")
        return saldo, extrato

    def sacar(self, *, valor, saldo, extrato=None):
        if self.saques_diarios >= self.limite_saques_diarios:
            print("Limite de saques diários atingido.")
        elif valor > self.limite_saque:
            print(f"Não é possível sacar mais do que R$ {self.limite_saque:.2f} por operação.")
        elif valor <= 0:
            print("Valor de saque inválido.")
        elif valor > saldo:
            print("Saldo insuficiente para realizar o saque.")
        else:
            saldo -= valor
            self.saques_diarios += 1
            if extrato is not None:
                extrato.append(f"Saque: R$ {valor:.2f}")
            print(f"Saque de R$ {valor:.2f} realizado com sucesso.")
        return saldo, extrato

    def exibir_extrato(self, saldo, *, extrato=None):
        if extrato is None or not extrato:
            print("Não foram realizadas movimentações.")
        else:
            for movimentacao in extrato:
                print(movimentacao)
        print(f"Saldo atual: R$ {saldo:.2f}")

    def resetar_saques_diarios(self):
        self.saques_diarios = 0


# Listas para armazenar usuários e contas correntes
usuarios = []
contas_correntes = []


def cadastrar_usuario(nome, data_nascimento, cpf, endereco):
    # Verifica se o CPF já está cadastrado
    for usuario in usuarios:
        if usuario.cpf == cpf:
            print(f"CPF {cpf} já cadastrado. Não é possível cadastrar o usuário.")
            return None
    
    # Adiciona o usuário à lista de usuários
    novo_usuario = Usuario(nome, data_nascimento, cpf, endereco)
    usuarios.append(novo_usuario)
    print(f"Usuário {nome} cadastrado com sucesso!")
    return novo_usuario


def criar_conta_corrente(usuario):
    # Cria uma nova conta corrente para o usuário
    nova_conta = ContaCorrente(usuario)
    contas_correntes.append(nova_conta)
    print(f"Conta corrente criada para o usuário {usuario.nome}. Número da conta: {nova_conta.numero_conta}")
    return nova_conta


# Exemplo de uso das funções
if __name__ == "__main__":
    # Cadastrando usuários
    usuario1 = cadastrar_usuario("João", "01/01/1990", "12345678900", "Rua Principal, Centro, SP")
    usuario2 = cadastrar_usuario("Maria", "15/05/1985", "98765432100", "Avenida Central, Bairro Novo, RJ")
    
    # Criando contas correntes
    if usuario1:
        conta1 = criar_conta_corrente(usuario1)
    if usuario2:
        conta2 = criar_conta_corrente(usuario2)
    
    # Realizando operações nas contas
    if usuario1:
        saldo1, extrato1 = conta1.depositar(0.0, 1000.0, extrato=[])
        saldo1, extrato1 = conta1.sacar(valor=500.0, saldo=saldo1, extrato=extrato1)
        conta1.exibir_extrato(saldo1, extrato=extrato1)
    
    if usuario2:
        saldo2, extrato2 = conta2.depositar(0.0, 1500.0, extrato=[])
        saldo2, extrato2 = conta2.sacar(valor=200.0, saldo=saldo2, extrato=extrato2)
        conta2.exibir_extrato(saldo2, extrato=extrato2)
Explicação do Código:
Classe Usuario:

Representa um usuário com atributos como nome, data de nascimento, CPF e endereço.
Classe ContaCorrente:

Representa uma conta corrente com atributos como usuário associado, número da conta, saldo, movimentações, limites de saque diários e máximo por operação.
Métodos depositar, sacar, exibir_extrato e resetar_saques_diarios para operações bancárias, recebendo argumentos conforme especificado (positional only para depósito, keyword only para saque).
Funções cadastrar_usuario e criar_conta_corrente:

cadastrar_usuario cria um novo objeto Usuario e o adiciona à lista de usuários se o CPF não estiver duplicado.
criar_conta_corrente cria uma nova conta corrente para um usuário especificado e a adiciona à lista de contas correntes.
Exemplo de Uso:

Cadastra dois usuários.
Cria uma conta corrente para cada usuário.
Realiza operações como depósito e saque nas contas correntes.
Exibe o extrato atual das contas após as operações.
Esse código organiza as funcionalidades de maneira mais modular e separada, seguindo as especificações fornecidas. Cada função e método tem um propósito claro e utiliza os argumentos de entrada de acordo com as necessidades específicas de cada operação.
