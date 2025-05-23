# Alocação automática dos atendentes

Essa página é dedicada para usuários que desejam configurar uma alocação automática dos chamados para seus atendentes.

Existem duas formas de alocação: uniforme e ponderada

## Definições

- **Alocação uniforme**
    
    Não leva em consideração o peso de um chamado. Ou seja, se existem dois atendentes numa fila, então os chamados serão distribuídos para ambos considerando apenas a quantidade de chamados com cada atendente.
    
- **Alocação ponderada**
    
    Leva em consideração um campo do formulário, que indica a complexidade (peso) do chamado, na hora de distribuir o chamado. Ou seja, se existem dois atendentes numa fila, porém o atendente A possui apenas um chamado com complexidade (peso) igual a 20, enquanto o atendente B possui dois chamados, porém o somatório das complexidades (peso) é 18. Então o próximo chamado será atribuído para B mesmo este tendo maior quantidade de chamados.
    

## Como preencher o database ticketAllocationRules

Todos os casos de uso são configurados no database `ticketAllocationRules`. **Ao preencher um item desse database, devemos ter em mente o funcionamento básico do algoritmo de distribuição**

### Algoritmo utilizado

1. **Indicamos o gatilho da regra**
    1. Aqui podemos indicar se a regra será disparada quando houver uma criação de chamado da categoria x, ou atualização do chamado de categoria y…
2. **Listamos os possíveis candidatos a responsáveis do chamado**
    1. Nesse passo conseguimos informar quais grupos a alocação deve ser feita, além de indicar skills (ainda não implementado) que os usuários devem ter para ser candidato a responsável do chamado.
3. **Listamos os chamados desse contexto da regra e calculamos seu peso.** Caso não seja informado o campo peso, então cada chamado terá peso 1, em outras palavras, a atribuição será quantitativa (uniforme)
4. **Por default é criado uma action com type allocationRules-${[tua.id](http://tua.id/)},** lembrando que a configuração de layout dessa action pode ser feita da seguinte forma
    
    No database ticketAllocationRules:
    
    ![image.png](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F7d498dc262919d592826dc5e50c6d714.png?alt=media&token=1add3a45-0116-4248-b205-f0d44e82f2bf)
    
    ou no json da categoria/tipo:
    
    ![image.png](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F0611dd15b2d7c01967228965b1c71bd6.png?alt=media&token=9a14f801-d9a7-4686-8f68-073822f0e32a)
    

## Associação dessa regra no ticketType ou category

![image.png](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F6dbc1472e67ca09bba7935e94c3272dc.png?alt=media&token=63b248f7-bf80-47c3-90d9-59ecf7d0403b)

## Principais casos de uso

### Atribuição uniforme focada na igualdade do atendentes

Essa configuração é útil quando queremos garantir que todos os atendentes recebam a mesma quantidade de chamados independentemente de isso implicar em mais tempo de espera do chamado para ser atendido

![image.png](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2Fbd76df4c067ea9a80c33b31363f1328b.png?alt=media&token=3e127f2e-607b-46ed-9de6-778569d71796)

### Atribuição uniforme focado na maior quantidade de chamados atendidos

![image.png](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2Fc86b45002739db02adc5be8200c96b28.png?alt=media&token=bd3685fc-74ee-419c-98f3-b9bd907579f4)

![image.png](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F4ede6e81fbeab6f0a6dc41b78f20b1ae.png?alt=media&token=899a33f4-671a-4823-89ea-3c9e5ece13e0)

### Atribuição ponderada

![image.png](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F44487ae4ea50d33bfe3ea48678ace95d.png?alt=media&token=02d39e54-a5f8-4337-baec-f020c174b11a)

![image.png](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F5fa6604d098ec0d19d10fe9bdbdb9118.png?alt=media&token=af5e1107-efc8-42a7-8f5b-5489b8f68fb4)