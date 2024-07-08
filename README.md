# Canivete suíço testes

## Testando frase dentro de um componente

`//importando lib
import { render, screen } from '@testing-library/react';

//importando componente
import Menu from './index';

test('deve renderizar um link para página inicial', () => {
render(<Menu />);
const linkPaginaInicial = screen.getByText('Inicial');
expect(linkPaginaInicial).toBeInTheDocument();
});`

## Testando um conjunto de links

`test('deve renderizar uma série de links', () => {
render(<Menu />);
//encontram os links
const listaLinks = screen.getAllByRole('link');

//espera-se 4 links
expect(listaLinks).toHaveLength(4);
});`

## Esperando que um link não apareça

`test("Não deve realizar o link para extrato", () => {
render(<Menu />);
//encontre a palavra/link Extrato
const linkExtrato = screen.queryByText('Extrato');
//Espero não encontrar o LinkExtrato
expect(linkExtrato).not.toBeInTheDocument()
});`

## Testando uma série de links com a classe de nome link

`test('deve renderizar uma lista de link com a classe link', () => {
render(<Menu />);
const links = screen.getAllByRole('link');
links.forEach((link) => expect(link).toHaveClass('link'));
});`

## Testando um placeholder de input

`test('deve renderizar um campo de input', () => {
render(<Formulario />);
const campoTexto = screen.getByPlaceholderText('Digite um valor');
expect(campoTexto).toBeInTheDocument();
});`

## Testando um input type number

`test('deve renderizar um campo de input number', () => {
  render(<Formulario />);
  const campoTexto = screen.getByPlaceholderText('Digite um valor');
  expect(campoTexto).toHaveAttribute('type', 'number');
});`

## Renderizando uma lista de transações, recebendo um parâmetro dentro do componente

`test('Deve renderizar uma lista de transações', () => {
  const transacoes = [
    {
      transacao: 'Depósito',
      valor: 100,
    },
  ];
  render(<Extrato transacoes={transacoes} />);
  //se usar role list ele vai pegar a role da lista, o correto é usar listitem (pegando os items que tem dentro da lista)
  const lista = screen.getByRole('listitem');
  expect(lista).toBeInTheDocument();
});`

## Renderizando componente recebendo valor e depósito com uso de uma data-testid, renderizando sempre novas propos

`test('Deve renderizar mesmo componente com props atualizadas', () => {
const transacao = {
transacao: 'Depósito',
valor: 100,
};
const { rerender } = render(
<Transacoes estilos={estilos} transacao={transacao} />,
);
const tipoTransacao = screen.getByTestId('tipoTransacao');
const valorTransacao = screen.getByTestId('valorTransacao');

expect(tipoTransacao).toHaveTextContent('Depósito');
expect(valorTransacao).toHaveTextContent('R$ 100');

const novaTransacao = {
transacao: 'Transferência',
valor: 50,
};

rerender(<Transacoes estilos={estilos} transacao={novaTransacao} />);
const novoTipoTranserencia = screen.getByTestId('tipoTransacao');
const novoValorTransferencia = screen.getByTestId('valorTransacao');

expect(novoTipoTranserencia).toHaveTextContent('Transferência');
expect(novoValorTransferencia).toHaveTextContent('- R$ 50');
});`

# Testa um botão (onSubmit), deverá ser clicado ao menos uma vez

`test('Deve chamar um evento de onSubmit ao clicar em realizar transação', () => {
//mokando
const realizarTransacao = jest.fn();
render(<Formulario realizarTransacao={realizarTransacao} />);
const botao = screen.getByRole('button');

useEvent.click(botao);
//botão chamado no mínimo uma vez
expect(realizarTransacao).toHaveBeenCalledTimes(1);
});`

## Validado transação de transferência e depósitos

`describe('Quando eu realizo uma transação', () => {
test('Que é um depósito, o saldo aumenta', () => {
const transacao = {
transacao: 'Depósito',
valor: 50,
};
const novoSaldo = calculaNovoSaldo(transacao, 100);
expect(novoSaldo).toBe(150);
});

test('Que é uma transferência, o saldo diminui', () => {
const transacao = {
transacao: 'Transferência',
valor: 50,
};
const novoSaldo = calculaNovoSaldo(transacao, 100);
expect(novoSaldo).toBe(50);
});
});

test('Deve retornar o valor do saldo com rendimento', () => {
const calculaRendimento = jest.fn((saldo) => saldo + saldo \* 0.005);
const saldo = 100;

const novoSaldo = calculaRendimento(saldo);
expect(novoSaldo).toBe(100.5);

//esperando a função ser chamada
expect(calculaRendimento).toBeCalled();

//se ela foi chamado com parâmetro saldo
expect(calculaRendimento).toBeCalledWith(saldo);
});`
