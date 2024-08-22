<script>
  import Button from "$lib/components/ui/button/button.svelte";
  import Input from "$lib/components/ui/input/input.svelte";
  import Separator from "$lib/components/ui/separator/separator.svelte";
  import Label from "$lib/components/ui/label/label.svelte";
  import * as Card from "$lib/components/ui/card/index";
  import "./app.css";
  import PhoneInput from "$lib/components/ui/phone-input/phone-input.svelte";
  import WhatsAppIcon from "$lib/components/whatsapp-icon.svelte";
  import Textarea from "$lib/components/ui/textarea/textarea.svelte";
  import * as Popover from "$lib/components/ui/popover";
  import { CircleHelp } from "lucide-svelte";
  import { onMount } from "svelte";

  $: telefone = null;
  $: nome = null;
  $: mensagem = "";

  const openWhatsappChat = () => {
    let whatsChatUrl = new URL("https://wa.me/");
    if (telefone) whatsChatUrl.searchParams.append("phone", telefone);
    if (mensagemFinal) whatsChatUrl.searchParams.append("text", mensagemFinal);
    console.log(whatsChatUrl.toString());
    //window.open(``)
  };

  // Função para obter o parâmetro 'message' da URL
  const getMessageFromUrl = () => {
    const params = new URLSearchParams(window.location.search);
    return params.get("message") || "";
  };

  // Atualiza a URL com o valor do campo de mensagem
  const updateUrl = () => {
    const params = new URLSearchParams(window.location.search);
    params.set("message", mensagem);
    window.history.replaceState(
      {},
      "",
      `${window.location.pathname}?${params}`,
    );
  };

  // Preenche o campo de mensagem com o valor da URL ao carregar a página
  onMount(() => {
    mensagem = getMessageFromUrl();
  });

  $: mensagemFinal = mensagem
    .replaceAll("{{TELEFONE}}", telefone)
    .replaceAll("{{NOME}}", nome);
</script>

<main
  class="flex flex-col w-screen h-screen gap-8 px-8 py-4 items-center justify-center"
>
  <Card.Root class="w-[650px]">
    <Card.Header class="flex justify-center items-center">
      <Card.Title>Gerador de Link para chat do WhatsApp</Card.Title>
    </Card.Header>
    <Separator class="mb-6" />
    <Card.Content class="flex flex-col justify-center items-stretch gap-4">
      <div class="flex justify-center items-center gap-4">
        <div class="flex w-full max-w-sm flex-col gap-1.5">
          <Label for="phone" class="flex justify-between">
            Número de Telefone:

            <Popover.Root>
              <Popover.Trigger>
                <CircleHelp class="size-4" />
              </Popover.Trigger>
              <Popover.Content>
                <p>
                  Use a variável <b>&#123;&#123;TELEFONE&#125;&#125;</b> para ser
                  substituído pelo telefone que você informar no campo.
                </p>
              </Popover.Content>
            </Popover.Root>
          </Label>
          <PhoneInput defaultCountry="BR" name="phone" bind:value={telefone} />
        </div>
        <div class="flex w-full max-w-sm flex-col gap-1.5">
          <Label for="name" class="flex justify-between">
            Nome do Meliante:

            <Popover.Root>
              <Popover.Trigger>
                <CircleHelp class="size-4" />
              </Popover.Trigger>
              <Popover.Content>
                <p>
                  Use a variável <b>&#123;&#123;NOME&#125;&#125;</b> para ser substituído
                  pelo nome que você informar no campo.
                </p>
              </Popover.Content>
            </Popover.Root>
          </Label>
          <Input type="text" name="name" bind:value={nome} />
        </div>
      </div>
      <div>
        <div class="flex w-full flex-col gap-1.5">
          <Label for="msg">Mensagem:</Label>
          <Textarea
            rows={7}
            name="msg"
            class="resize-none"
            on:input={updateUrl}
            bind:value={mensagem}
          />
        </div>
      </div>
      <div>
        <div class="flex w-full flex-col gap-1.5">
          <Label for="msg-final">Mensagem Final:</Label>
          <Textarea
            rows={7}
            name="msg-final"
            disabled
            class="resize-none"
            bind:value={mensagemFinal}
          />
        </div>
      </div>
    </Card.Content>
    <Separator class="mb-6" />
    <Card.Footer class="flex justify-center items-center">
      <Button
        type="submit"
        class="flex gap-1 bg-green-700 hover:bg-green-800"
        disabled={!telefone}
        on:click={openWhatsappChat}
      >
        <WhatsAppIcon />
        Abrir Conversa no WhatsApp
      </Button>
    </Card.Footer>
  </Card.Root>
</main>
