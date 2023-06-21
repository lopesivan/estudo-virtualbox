Sim, é possível passar parâmetros para a inicialização de uma máquina virtual com Linux no VirtualBox usando a biblioteca VBoxJavaAPI. Você pode definir opções do GRUB ou até mesmo simular a inserção de uma senha no momento do login.

Aqui está um exemplo de como você pode definir opções do GRUB e inserir uma senha durante a inicialização de uma máquina virtual:

```java
import org.virtualbox_6_1.*;

public class VirtualBoxExample {

    public static void main(String[] args) {
        // Inicializando a API do VirtualBox
        VirtualBoxManager vboxManager = VirtualBoxManager.createInstance(null);

        try {
            // Abrindo a conexão com o VirtualBox
            vboxManager.connect("");

            // Obtendo o objeto de referência para o gerenciador do VirtualBox
            IVirtualBox vbox = vboxManager.getVBox();

            // Nome da máquina virtual que você deseja iniciar
            String vmName = "NomeDaSuaMaquinaVirtual";

            // Obtendo o objeto de referência para a máquina virtual
            IMachine vm = vbox.findMachine(vmName);

            // Inicializando a máquina virtual em uma sessão
            ISession session = vboxManager.getSessionObject();

            // Configurando as opções do GRUB
            String[] grubOptions = {
                "quiet",      // Modo silencioso
                "splash",     // Exibir tela de splash
                "vga=791"     // Configuração de vídeo
            };
            vm.setExtraData("VBoxInternal/Devices/pcbios/0/Config/Lilo/CommandLine", String.join(" ", grubOptions));

            // Inserindo uma senha no momento do login
            String password = "senha123";
            IGuest guest = vm.getGuest();
            IProgress progress = guest.login(session, "Usuário", password, null);

            // Aguardando a conclusão do processo de login
            progress.waitForCompletion(-1);

            // Verificando se o login foi bem-sucedido
            if (progress.getResultCode() == 0) {
                System.out.println("Login bem-sucedido!");
            } else {
                System.out.println("Falha no login. Código de erro: " + progress.getResultCode());
            }

            // Liberando a sessão e desconectando do VirtualBox
            session.unlockMachine();
            vboxManager.disconnect();
        } catch (VBoxException e) {
            e.printStackTrace();
        } finally {
            // Finalizando a API do VirtualBox
            vboxManager.cleanup();
        }
    }
}
```

No exemplo acima, antes de iniciar a máquina virtual, configuramos as opções do GRUB usando o método `setExtraData()` da classe `IMachine`. Em seguida, utilizamos o método `login()` da classe `IGuest` para simular a inserção de uma senha no momento do login. A senha é especificada como uma String na variável `password`.

Lembre-se de substituir "NomeDaSuaMaquinaVirtual" pelo nome correto da sua máquina virtual e ajustar o nome de usuário e senha conforme necessário.

Esses são apenas exemplos simples para demonstrar como passar parâmetros durante a inicialização de uma máquina virtual com Linux no VirtualBox usando a biblioteca VBoxJavaAPI. Você pode adaptar esses exemplos e explorar outras opções disponíveis na API para atender às suas necessidades específicas.
