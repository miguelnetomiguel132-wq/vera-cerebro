exports.handler = async function (event) {
  try {
    const { texto } = JSON.parse(event.body);

    const resposta = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "x-api-key": process.env.ANTHROPIC_API_KEY,
        "anthropic-version": "2023-06-01"
      },
      body: JSON.stringify({
        model: "claude-sonnet-4-6",
        max_tokens: 200,
        system:
          'Interpretas comandos de voz em português para um assistente que controla um telemóvel. ' +
          'Responde SEMPRE e APENAS com um JSON válido, sem markdown, sem texto antes ou depois, no formato: ' +
          '{"acao": "ligar" | "mensagem" | "abrir" | "desconhecido", "alvo": "nome da pessoa ou app", "texto": "conteudo da mensagem, se houver"}. ' +
          'Exemplo: comando "liga para a Ana" -> {"acao":"ligar","alvo":"Ana","texto":""}. ' +
          'Exemplo: comando "manda mensagem para a Ana a dizer já vou" -> {"acao":"mensagem","alvo":"Ana","texto":"já vou"}. ' +
          'Exemplo: comando "abre o whatsapp" -> {"acao":"abrir","alvo":"whatsapp","texto":""}.',
        messages: [{ role: "user", content: texto }]
      })
    });

    const data = await resposta.json();
    const textoResposta = data.content?.[0]?.text?.trim() || '{"acao":"desconhecido"}';

    return {
      statusCode: 200,
      headers: { "Content-Type": "application/json" },
      body: textoResposta
    };
  } catch (erro) {
    return {
      statusCode: 500,
      body: JSON.stringify({ acao: "erro", detalhe: erro.message })
    };
  }
};
