# Métodos join e sleep da classe Thread
## Introdução

Uma thread é uma unidade de execução independente dentro de um programa, a qual permite realizarmos mais de uma tarefa ao mesmo tempo.
Porém, em algum momento podemos precisar atrasar uma execução, ou uma thread tem dependência de outra e se faz necessário utilizar alguns métodos para realizar isso. 
Resolvemos essas questões com os métodos join e sleep.

## Utilidades e diferenças entre ```join``` e ```sleep```

```Join```

Esse método permite que uma thread espere pela concusão da outra antes de continuar, o que a torna mais útil quando você necessita garantir que a tarefa só vai ser continuada quando a outra terminar. 

```sleep```

O método sleep tem uma funcionalidade parecida, porém a execução do método em questão será continuada após o termino do tempo estipulado previamente. Esse método é mais utilizado para criar intervalos de tempo, simular atrasos ou até mesmo liberar o processador temporariamente.


## Código exemplo

```java

public class ThreadExample {

    public static void main(String[] args) {
        // Criando a primeira thread
        Thread thread1 = new Thread(() -> {
            try {
                for (int i = 1; i <= 5; i++) {
                    System.out.println("Thread 1 - Contagem: " + i);
                    Thread.sleep(1000); // Pausa por 1 segundo
                }
            } catch (InterruptedException e) {
                System.out.println("Thread 1 foi interrompida.");
            }
        });

        // Criando a segunda thread
        Thread thread2 = new Thread(() -> {
            try {
                System.out.println("Thread 2 aguardando a conclusão de Thread 1...");
                thread1.join(); // Aguarda a conclusão de thread1
                for (int i = 1; i <= 5; i++) {
                    System.out.println("Thread 2 - Contagem: " + i);
                    Thread.sleep(500); // Pausa por 0.5 segundos
                }
            } catch (InterruptedException e) {
                System.out.println("Thread 2 foi interrompida.");
            }
        });

        // Iniciando as threads
        thread1.start();
        thread2.start();

        // Mensagem final após conclusão de todas as threads
        try {
            thread1.join(); // Garantir que thread1 finalize antes de continuar
            thread2.join(); // Garantir que thread2 finalize antes de continuar
        } catch (InterruptedException e) {
            System.out.println("Main thread foi interrompida.");
        }

        System.out.println("Todas as threads finalizaram. Programa encerrado.");
    }
}

```

## Código explicado

Thread 1:

Faz uma contagem de 1 a 5, pausando por 1 segundo entre cada número usando o método sleep().
Representa uma tarefa que leva mais tempo para ser concluída.

Thread 2:

Aguarda a conclusão da Thread 1 usando o método join().
Após a Thread 1 terminar, inicia sua própria contagem de 1 a 5, pausando por 0.5 segundos entre cada número.

```thread1.join(); ```

Garante que thread1 finalize antes de continuar

```thread2.join(); ```

Garante que thread2 finalize antes de continuar


## Saída esperada

```java

Thread 1 - Contagem: 1
Thread 1 - Contagem: 2
Thread 1 - Contagem: 3
Thread 1 - Contagem: 4
Thread 1 - Contagem: 5
Thread 2 aguardando a conclusão de Thread 1...
Thread 2 - Contagem: 1
Thread 2 - Contagem: 2
Thread 2 - Contagem: 3
Thread 2 - Contagem: 4
Thread 2 - Contagem: 5
Todas as threads finalizaram. Programa encerrado.

```

## Saída sem ``` join ```

```java

Todas as threads finalizaram. Programa encerrado.
Thread 2 - Contagem: 1
Thread 1 - Contagem: 1
Thread 2 - Contagem: 2
Thread 1 - Contagem: 2
Thread 2 - Contagem: 3
Thread 2 - Contagem: 4
Thread 2 - Contagem: 5
Thread 1 - Contagem: 3
Thread 1 - Contagem: 4
Thread 1 - Contagem: 5

```


## Referências

https://www.youtube.com/watch?v=-PNCVYsmcrM&ab_channel=learnbybhanu - join() and sleep() Method in Thread 

https://learn.microsoft.com/pt-br/dotnet/api/system.threading.thread.join?view=net-8.0 - Thread.Join Método


