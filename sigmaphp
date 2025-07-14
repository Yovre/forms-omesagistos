<?php
// Define que a resposta será no formato JSON
header('Content-Type: application/json');

// Inclui o arquivo de conexão
include 'conexao.php';

// Cria um array para a resposta
$response = [
    'success' => false,
    'message' => 'Ocorreu um erro desconhecido.'
];

// Pega os dados enviados pelo JavaScript
$nome = $_POST['nome'] ?? '';
$idade = $_POST['idade'] ?? 0;
$discord = $_POST['discord'] ?? '';
$motivo = $_POST['motivo'] ?? '';

// Validação simples no servidor (importante, nunca confie 100% no JS)
if (empty($nome) || empty($idade) || empty($discord) || empty($motivo)) {
    $response['message'] = 'Por favor, preencha todos os campos.';
    echo json_encode($response);
    exit(); // Para a execução do script
}

// Prepara o comando SQL de forma segura
$sql = "INSERT INTO candidaturas (nome, idade, discord, motivo) VALUES (?, ?, ?, ?)";
$stmt = $conexao->prepare($sql);

if ($stmt) {
    // "siss" = String, Integer, String, String
    $stmt->bind_param("siss", $nome, $idade, $discord, $motivo);

    // Executa e atualiza a resposta
    if ($stmt->execute()) {
        $response['success'] = true;
        $response['message'] = 'Aplicação enviada com sucesso! Agradecemos o seu interesse.';
    } else {
        $response['message'] = 'Erro ao salvar os dados no banco: ' . $stmt->error;
    }
    $stmt->close();
} else {
    $response['message'] = 'Erro ao preparar a consulta: ' . $conexao->error;
}

$conexao->close();

// Imprime a resposta em formato JSON para o JavaScript ler
echo json_encode($response);
?>
