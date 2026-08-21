from datetime import datetime
# =============================================================
# ORGANIZAÇÃO DA LOJA
# =============================================================
class Produto:
    def __init__(self, codigo, nome, preco, estoque, categoria):
        self.codigo = codigo
        self.nome = nome
        self.preco = float(preco)
        self.estoque = int(estoque)
        self.categoria = categoria

    def __str__(self):
        return (
            f"{self.codigo} - {self.nome} / "
            f"R$ {self.preco:.2f} / "
            f"Estoque: {self.estoque} / "
            f"Categoria: {self.categoria}"
        )
# =============================================================
# DADOS DOS CLIENTES
# =============================================================
class Cliente:
    def __init__(self, codigo, nome, endereco, telefone):
        self.codigo = codigo
        self.nome = nome
        self.endereco = endereco
        self.telefone = telefone

    def __str__(self):
        return (
            f"{self.codigo} - {self.nome} / "
            f"{self.endereco} / {self.telefone}"
        )
# =============================================================
# DADOS DOS PEDIDOS
# =============================================================
class Pedido:
    def __init__(self, numero, data, cliente, itens):
        self.numero = numero
        self.data = data
        self.cliente = cliente
        self.itens = itens
        self.total = self.calcular_total()

    def calcular_total(self):
        total = 0.0

        for item in self.itens:
            produto = item["produto"]
            quantidade = item["quantidade"]

            total += produto.preco * quantidade

        return round(total, 2)
  # =============================================================
  # HORÁRIO DE REALIAÇÃO DO PEDIDO
  # =============================================================
    def __str__(self):
        texto = (
            f"Pedido #{self.numero} - "
            f"Cliente: {self.cliente.nome} - "
            f"Data: {self.data.strftime('%d/%m/%Y %H:%M')}\n")


        for item in self.itens:
            produto = item["produto"]
            qtd = item["quantidade"]

            texto += (
                f" - {produto.nome} x{qtd} = "
                f"R$ {produto.preco * qtd:.2f}\n"
            )

        texto += f"Total: R$ {self.total:.2f}"

        return texto


class CoffeShopTiaRosa:

    def __init__(self):
        self.produtos = []
        self.clientes = []
        self.pedidos = []
        self.proximo_numero_pedido = 1

        self._carregar_dados_iniciais()

    # =========================================================
    # DADOS DOS PRODUTOS
    # =========================================================

    def _carregar_dados_iniciais(self):

        self.produtos.append(
            Produto(
                "001",
                "Café tradicional",
                2.00,
                20,
                "Bebida"
            )
        )

        self.produtos.append(
            Produto(
                "002",
                "Café com leite",
                5.00,
                20,
                "Bebida"
            )
        )

        self.produtos.append(
            Produto(
                "003",
                "Cappuccino",
                7.50,
                20,
                "Bebida"
            )
        )

        self.produtos.append(
            Produto(
                "004",
                "Pão de Queijo",
                3.50,
                20,
                "Café da manhã"
            )
        )

        self.produtos.append(
            Produto(
                "005",
                "Bolo de chocolate",
                7.00,
                20,
                "Doce"
            )
        )

        self.clientes.append(
            Cliente("CLI001", "Guilherme Matheus", "Rua A, 123","610000001"
            )
        )

        self.clientes.append(
            Cliente(
                "CLI002",
                "Geovana Flor",
                "Rua B, 456",
                "610000002"
            )
        )

        self.clientes.append(
            Cliente(
                "CLI003",
                "Maria Antonia",
                "Rua C, 789",
                "610000003"
            )
        )

    # =========================================================
    # LAYOUT DO MENU PRINCIPAL
    # =========================================================

    def executar(self):

        while True:

            print("\n" + "~" * 40)
            print("        CAFÉ - TIA ROSA - SYSTEM")
            print("~" * 40)

            print("1. Cadastrar produto")
            print("2. Listar produtos")
            print("3. Cadastrar cliente")
            print("4. Listar clientes")
            print("5. Fazer pedido")
            print("6. Listar pedidos")
            print("7. Relatório de vendas")
            print("0. Sair")

            opcao = input("\nEscolha uma opção: ").strip()

            if opcao == "1":
                self.cadastrar_produto()

            elif opcao == "2":
                self.listar_produtos()

            elif opcao == "3":
                self.cadastrar_cliente()

            elif opcao == "4":
                self.listar_clientes()

            elif opcao == "5":
                self.fazer_pedido()

            elif opcao == "6":
                self.listar_pedidos()

            elif opcao == "7":
                self.relatorio_vendas()

            elif opcao == "0":
                print("\nObrigado por usar TIA ROSA SYSTEM!")
                break

            else:
                print("\nOpção inválida. Tente novamente.")

    # =========================================================
    # BUSCAR PRODUTO POR CÓDIGO
    # =========================================================

    def buscar_produto_por_codigo(self, codigo):

        for produto in self.produtos:

            if produto.codigo.upper() == codigo.upper():
                return produto

        return None

    # =========================================================
    # BUSCAR CLIENTE POR TELEFONE
    # =========================================================

    def buscar_cliente_por_telefone(self, telefone):

        for cliente in self.clientes:

            if cliente.telefone == telefone:
                return cliente

        return None

    # =========================================================
    # CADASTRAR PRODUTO
    # =========================================================

    def cadastrar_produto(self):

        print("\n" + "=" * 40)
        print("CADASTRO DE PRODUTO")
        print("=" * 40)

        codigo = input(
            "Digite o código do produto: "
        ).strip().upper()

        nome = input(
            "Digite o nome do produto: "
        ).strip()

        if self.buscar_produto_por_codigo(codigo):

            print(
                "\nJá existe um produto com esse código."
            )

            return

        try:

            preco = float(
                input(
                    "Digite o preço do produto: "
                ).replace(",", ".")
            )

            estoque = int(
                input("Estoque inicial: ")
            )

        except ValueError:

            print(
                "\nPreço ou estoque inválido. "
                "Tente novamente."
            )

            return

        if preco <= 0 or estoque < 0:

            print(
                "\nPreço deve ser maior que zero "
                "e estoque não pode ser negativo."
            )

            return

        categoria = input(
            "Digite a categoria do produto: "
        ).strip()

        novo_produto = Produto(
            codigo,
            nome,
            preco,
            estoque,
            categoria
        )

        self.produtos.append(novo_produto)

        print("\nProduto cadastrado com sucesso!")

    # =========================================================
    # LISTAR PRODUTOS
    # =========================================================

    def listar_produtos(self):

        print("\n" + "=" * 70)
        print("                         LISTA DE PRODUTOS")
        print("=" * 70)

        if not self.produtos:

            print("Nenhum produto cadastrado.")
            return

        for produto in self.produtos:
            print(produto)

    # =========================================================
    # CADASTRAR CLIENTE
    # =========================================================

    def cadastrar_cliente(self):

        print("\n" + "=" * 40)
        print("          CADASTRO DE CLIENTE")
        print("=" * 40)

        nome = input(
            "Digite o nome do cliente: "
        ).strip()

        endereco = input(
            "Digite o endereço do cliente: "
        ).strip()

        telefone = input(
            "Digite o telefone do cliente: "
        ).strip()

        if not nome or not telefone or not endereco:

            print(
                "\nNome, endereço e telefone "
                "são campos obrigatórios."
            )

            return

        if self.buscar_cliente_por_telefone(telefone):

            print(
                "\nJá existe um cliente com esse telefone."
            )

            return

        codigo = f"CLI{len(self.clientes) + 1:03d}"

        cliente = Cliente(
            codigo,
            nome,
            endereco,
            telefone
        )

        self.clientes.append(cliente)

        print("\nCliente cadastrado com sucesso!")

    # =========================================================
    # LISTAR CLIENTES
    # =========================================================

    def listar_clientes(self):

        print("\n" + "=" * 70)
        print("LISTA DE CLIENTES")
        print("=" * 70)

        if not self.clientes:

            print("Nenhum cliente cadastrado.")
            return

        for cliente in self.clientes:
            print(cliente)

    # =========================================================
    # FAZER PEDIDO
    # =========================================================

    def fazer_pedido(self):

        print("\n")
        print("=" * 55)
        print("FAZER PEDIDO")
        print("=" * 55)

        if not self.clientes:

            print(
                "Cadastre pelo menos um cliente "
                "antes de iniciar."
            )

            return
      # =============================================================
      # MOSTRAR CLEINTES DISPONÍVEIS
      # =============================================================
        self.listar_produtos()

        telefone_cliente_input = input(
            "\nDigite o telefone do cliente: "
        ).strip()

        cliente = self.buscar_cliente_por_telefone(
            telefone_cliente_input
        )

        if not cliente:

            print("\nCliente não encontrado.")
            return
    # =============================================================
    # MOSTRAR PRODUTO IDENTIFICADO
    # =============================================================
        print("\n" + "-" * 55)
        print("CLIENTE SELECIONADO")
        print("-" * 55)
        print(f"Nome: {cliente.nome}")
        print(f"Telefone: {cliente.telefone}")
        print(f"Endereço: {cliente.endereco}")
        print("-" * 55)

        itens_pedido = []

        while True:

            codigo_produto = input(
                "\nDigite o código do produto "
                "(ou 0 para sair): "
            ).strip().upper()

            if codigo_produto == "0":
                break

            produto = self.buscar_produto_por_codigo(
                codigo_produto
            )

            if not produto:

                print("Produto não encontrado.")
                continue

            if produto.estoque == 0:

                print("Produto sem estoque.")
                continue

            try:

                quantidade = int(
                    input("Quantidade: ")
                )

            except ValueError:

                print(
                    "Quantidade inválida. "
                    "Tente novamente."
                )

                continue

            if quantidade <= 0:

                print(
                    "Quantidade deve ser maior que zero."
                )

                continue

            if quantidade > produto.estoque:

                print(
                    "Quantidade maior que "
                    "disponível em estoque."
                )

                continue

            # Retira do estoque
            produto.estoque -= quantidade

            # Adiciona ao pedido
            itens_pedido.append(
                {
                    "produto": produto,
                    "quantidade": quantidade
                }
            )

            subtotal = produto.preco * quantidade

            print(
                f"\nItem adicionado: "
                f"{produto.nome} x{quantidade} "
                f"= R$ {subtotal:.2f}"
            )

            continuar = input(
                "Deseja adicionar mais itens? (S/N): "
            ).strip().lower()

            if continuar != "s":
                break

        # Se nenhum item foi adicionado
        if not itens_pedido:

            print(
                "\nPedido cancelado. "
                "Nenhum item adicionado."
            )

            return

        # Cria o pedido
        pedido = Pedido(
            self.proximo_numero_pedido,
            datetime.now(),
            cliente,
            itens_pedido
        )

        self.pedidos.append(pedido)

        self.proximo_numero_pedido += 1

        # =====================================================
        # PEDIDO SEPARADO
        # =====================================================

        print("\n\n")
        print("=" * 60)
        print("PEDIDO REALIZADO COM SUCESSO")
        print("=" * 60)

        print(
            f"Pedido: #{pedido.numero}"
        )

        print(
            f"Cliente: {pedido.cliente.nome}"
        )

        print(
            f"Telefone: {pedido.cliente.telefone}"
        )

        print(
            f"Data: {pedido.data.strftime('%d/%m/%Y %H:%M')}"
        )

        print("\n" + "-" * 60)
        print("ITENS DO PEDIDO")
        print("-" * 60)

        for item in pedido.itens:

            produto = item["produto"]
            quantidade = item["quantidade"]

            subtotal = produto.preco * quantidade

            print(
                f"{produto.nome} "
                f"x{quantidade} "
                f"........ R$ {subtotal:.2f}"
            )

        print("-" * 60)

        print(
            f"TOTAL: R$ {pedido.total:.2f}"
        )

        print(
            "STATUS: Pendente"
        )

        print("=" * 60)

        print(
            "\nPedido adicionado à lista de pedidos."
        )

    # =========================================================
    # LISTAR PEDIDOS
    # =========================================================

    def listar_pedidos(self):

        print("\n\n")
        print("=" * 60)
        print("PEDIDOS REALIZADOS")
        print("=" * 60)

        if not self.pedidos:

            print("\nNenhum pedido encontrado.")
            print("=" * 60)

            return

        for pedido in self.pedidos:

            print("\n")
            print("=" * 60)

            print(
                f"PEDIDO #{pedido.numero}"
            )

            print(
                f"Cliente: {pedido.cliente.nome}"
            )

            print(
                f"Telefone: {pedido.cliente.telefone}"
            )

            print(
                f"Data: "
                f"{pedido.data.strftime('%d/%m/%Y %H:%M')}"
            )

            print("-" * 60)

            print("Itens:")

            for item in pedido.itens:

                produto = item["produto"]
                quantidade = item["quantidade"]

                subtotal = produto.preco * quantidade

                print(
                    f"  - {produto.nome} "
                    f"x{quantidade} "
                    f"........ R$ {subtotal:.2f}"
                )

            print("-" * 60)

            print(
                f"TOTAL: R$ {pedido.total:.2f}"
            )

            print(
                "STATUS: Pendente"
            )

        print("=" * 60)

    # =========================================================
    # RELATÓRIO DE VENDAS
    # =========================================================

    def relatorio_vendas(self):

        print("\n")
        print("=" * 50)
        print("RELATÓRIO DE VENDAS")
        print("=" * 50)

        if not self.pedidos:

            print("Nenhum pedido encontrado.")
            return

        total_vendas = sum(
            pedido.total
            for pedido in self.pedidos
        )

        print(
            f"Quantidade de pedidos: "
            f"{len(self.pedidos)}"
        )

        print(
            f"Total de vendas: "
            f"R$ {total_vendas:.2f}"
        )

        produtos_vendidos = {}

        for pedido in self.pedidos:

            for item in pedido.itens:

                produto = item["produto"]
                quantidade = item["quantidade"]

                produtos_vendidos[produto.nome] = (
                    produtos_vendidos.get(
                        produto.nome,
                        0
                    ) + quantidade
                )

        print("\nProdutos mais vendidos:")

        for produto, quantidade in produtos_vendidos.items():

            print(
                f"{produto}: "
                f"{quantidade} unidades"
            )


# =============================================================
# INICIAR SISTEMA
# =============================================================

if __name__ == "__main__":

    sistema = CoffeShopTiaRosa()
    sistema.executar()
