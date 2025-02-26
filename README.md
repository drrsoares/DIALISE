<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Assistente de Decisão para Diálise</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <style>
        :root {
            --primary-color: #5D5CDE;
            --secondary-color: #38B2AC;
            --warning-color: #FFC107;
            --danger-color: #DC3545;
            --success-color: #28A745;
            --info-color: #17A2B8;
        }
        
        /* Dark mode variables */
        @media (prefers-color-scheme: dark) {
            :root {
                --bg-color: #181818;
                --text-color: #f0f0f0;
                --card-bg: #2a2a2a;
                --border-color: #444;
                --input-bg: #333;
            }
        }
        
        /* Light mode variables */
        @media (prefers-color-scheme: light) {
            :root {
                --bg-color: #FFFFFF;
                --text-color: #333333;
                --card-bg: #FFFFFF;
                --border-color: #e0e0e0;
                --input-bg: #f7f7f7;
            }
        }
        
        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 1rem;
            transition: background-color 0.3s, color 0.3s;
        }
        
        .container {
            max-width: 1000px;
            margin: 0 auto;
            padding: 0 1rem;
        }
        
        .card {
            background-color: var(--card-bg);
            border-radius: 0.5rem;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            padding: 1.5rem;
            margin-bottom: 1.5rem;
            border: 1px solid var(--border-color);
            transition: box-shadow 0.3s ease;
        }
        
        .form-group {
            margin-bottom: 1rem;
        }
        
        .form-label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 500;
        }
        
        .form-control {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid var(--border-color);
            border-radius: 0.25rem;
            font-size: 1rem;
            background-color: var(--input-bg);
            color: var(--text-color);
            transition: border-color 0.2s ease-in-out;
        }
        
        .form-control:focus {
            border-color: var(--primary-color);
            outline: none;
            box-shadow: 0 0 0 2px rgba(93, 92, 222, 0.25);
        }
        
        .btn {
            padding: 0.75rem 1rem;
            border-radius: 0.25rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            border: none;
        }
        
        .btn-primary {
            background-color: var(--primary-color);
            color: white;
        }
        
        .btn-primary:hover {
            background-color: #4a49b9;
        }
        
        .btn-success {
            background-color: var(--success-color);
            color: white;
        }
        
        .btn-success:hover {
            background-color: #218838;
        }
        
        .btn-danger {
            background-color: var(--danger-color);
            color: white;
        }
        
        .btn-danger:hover {
            background-color: #c82333;
        }
        
        .btn-block {
            display: block;
            width: 100%;
        }
        
        .alert {
            padding: 1rem;
            border-radius: 0.25rem;
            margin-bottom: 1rem;
        }
        
        .alert-info {
            background-color: rgba(23, 162, 184, 0.2);
            border: 1px solid var(--info-color);
            color: var(--info-color);
        }
        
        .alert-warning {
            background-color: rgba(255, 193, 7, 0.2);
            border: 1px solid var(--warning-color);
            color: #856404;
        }
        
        .alert-danger {
            background-color: rgba(220, 53, 69, 0.2);
            border: 1px solid var(--danger-color);
            color: #721c24;
        }
        
        .alert-success {
            background-color: rgba(40, 167, 69, 0.2);
            border: 1px solid var(--success-color);
            color: #155724;
        }
        
        .text-center {
            text-align: center;
        }
        
        .flex-container {
            display: flex;
            gap: 1rem;
            margin-bottom: 1rem;
            flex-wrap: wrap;
        }
        
        .flex-container > div {
            flex: 1;
            min-width: 250px;
        }
        
        .hidden {
            display: none !important;
        }
        
        .form-check {
            display: flex;
            align-items: center;
            margin-bottom: 0.5rem;
        }
        
        .form-check-input {
            margin-right: 0.5rem;
            width: 1.25em;
            height: 1.25em;
        }
        
        .tabs {
            display: flex;
            border-bottom: 1px solid var(--border-color);
            margin-bottom: 1.5rem;
            overflow-x: auto;
        }
        
        .tab {
            padding: 0.75rem 1.25rem;
            cursor: pointer;
            transition: all 0.2s ease;
            border-bottom: 2px solid transparent;
            font-weight: 500;
            white-space: nowrap;
        }
        
        .tab.active {
            color: var(--primary-color);
            border-bottom: 2px solid var(--primary-color);
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
        
        @media (max-width: 768px) {
            .flex-container {
                flex-direction: column;
            }
            
            .flex-container > div {
                width: 100%;
            }
        }
        
        /* Spinner */
        .spinner {
            width: 40px;
            height: 40px;
            border: 4px solid rgba(93, 92, 222, 0.1);
            border-left-color: var(--primary-color);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 2rem auto;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        .result-card {
            border-left: 4px solid var(--primary-color);
            padding-left: 1rem;
        }
        
        .result-card.danger {
            border-left-color: var(--danger-color);
        }
        
        .result-card.warning {
            border-left-color: var(--warning-color);
        }
        
        .result-card.success {
            border-left-color: var(--success-color);
        }
        
        .result-card h3 {
            font-size: 1.25rem;
            margin-bottom: 0.5rem;
        }
        
        /* Range slider styling */
        input[type="range"] {
            -webkit-appearance: none;
            width: 100%;
            height: 8px;
            border-radius: 5px;
            background: var(--border-color);
            outline: none;
            margin-top: 0.5rem;
            margin-bottom: 1rem;
        }
        
        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: var(--primary-color);
            cursor: pointer;
        }
        
        input[type="range"]::-moz-range-thumb {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: var(--primary-color);
            cursor: pointer;
            border: none;
        }
        
        .range-labels {
            display: flex;
            justify-content: space-between;
            margin-top: -0.5rem;
            font-size: 0.8rem;
            color: var(--text-color);
            opacity: 0.7;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 1rem 0;
        }
        
        table, th, td {
            border: 1px solid var(--border-color);
        }
        
        th, td {
            padding: 0.75rem;
            text-align: left;
        }
        
        th {
            background-color: rgba(93, 92, 222, 0.1);
            font-weight: 600;
        }
        
        .prescription-section {
            background-color: rgba(40, 167, 69, 0.1);
            border: 1px solid var(--success-color);
            border-radius: 0.5rem;
            padding: 1rem;
            margin-top: 1rem;
        }
    </style>
</head>
<body>
    <div class="container">
        <header class="text-center my-6">
            <h1 class="text-2xl font-bold text-primary">Assistente de Decisão para Diálise</h1>
            <p class="text-sm opacity-75 mt-2">Avaliação de indicação e prescrição de terapia dialítica</p>
        </header>
        
        <div class="tabs">
            <div class="tab active" data-tab="dados-paciente">Dados do Paciente</div>
            <div class="tab" data-tab="parametros-laboratoriais">Parâmetros Laboratoriais</div>
            <div class="tab" data-tab="avaliacao-clinica">Avaliação Clínica</div>
            <div class="tab" data-tab="comorbidades">Comorbidades</div>
            <div class="tab" data-tab="resultado">Resultado</div>
        </div>
        
        <form id="dialysisForm">
            <!-- Dados do Paciente -->
            <div id="dados-paciente" class="tab-content active">
                <div class="card">
                    <h2 class="text-xl font-semibold mb-4">Dados do Paciente</h2>
                    
                    <div class="flex-container">
                        <div class="form-group">
                            <label for="nome" class="form-label">Nome do Paciente</label>
                            <input type="text" id="nome" class="form-control" placeholder="Digite o nome completo">
                        </div>
                        <div class="form-group">
                            <label for="registro" class="form-label">Registro Hospitalar</label>
                            <input type="text" id="registro" class="form-control" placeholder="Digite o registro">
                        </div>
                    </div>
                    
                    <div class="flex-container">
                        <div class="form-group">
                            <label for="idade" class="form-label">Idade (anos)</label>
                            <input type="number" id="idade" class="form-control" min="0" max="120">
                        </div>
                        <div class="form-group">
                            <label for="sexo" class="form-label">Sexo</label>
                            <select id="sexo" class="form-control">
                                <option value="">Selecione</option>
                                <option value="M">Masculino</option>
                                <option value="F">Feminino</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="flex-container">
                        <div class="form-group">
                            <label for="peso" class="form-label">Peso atual (kg)</label>
                            <input type="number" id="peso" class="form-control" min="0" step="0.1">
                        </div>
                        <div class="form-group">
                            <label for="altura" class="form-label">Altura (cm)</label>
                            <input type="number" id="altura" class="form-control" min="0" step="0.1">
                        </div>
                    </div>
                    
                    <div class="flex-container">
                        <div class="form-group">
                            <label for="pesoSeco" class="form-label">Peso seco estimado (kg)</label>
                            <input type="number" id="pesoSeco" class="form-control" min="0" step="0.1">
                        </div>
                        <div class="form-group">
                            <label for="acessoVascular" class="form-label">Acesso Vascular Disponível</label>
                            <select id="acessoVascular" class="form-control">
                                <option value="">Selecione</option>
                                <option value="cateter_temporario">Cateter Temporário para Diálise</option>
                                <option value="cateter_permanente">Cateter Permanente para Diálise</option>
                                <option value="fistula">Fístula Arteriovenosa</option>
                                <option value="sem_acesso">Sem acesso (a ser confeccionado)</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label for="nefropatiaBase" class="form-label">Nefropatia de Base</label>
                        <select id="nefropatiaBase" class="form-control">
                            <option value="">Selecione</option>
                            <option value="diabetica">Nefropatia Diabética</option>
                            <option value="hipertensiva">Nefropatia Hipertensiva</option>
                            <option value="glomerular">Doença Glomerular</option>
                            <option value="policistica">Doença Renal Policística</option>
                            <option value="indeterminada">Indeterminada</option>
                            <option value="outra">Outra</option>
                        </select>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Histórico de Terapia Renal Substitutiva</label>
                        <div class="form-check">
                            <input type="radio" id="trs_sim" name="historico_trs" class="form-check-input" value="sim">
                            <label for="trs_sim">Sim</label>
                        </div>
                        <div class="form-check">
                            <input type="radio" id="trs_nao" name="historico_trs" class="form-check-input" value="nao">
                            <label for="trs_nao">Não</label>
                        </div>
                    </div>
                    
                    <div id="historicoTRSDetails" class="hidden">
                        <div class="form-group">
                            <label for="tipoTRS" class="form-label">Tipo de TRS prévia</label>
                            <select id="tipoTRS" class="form-control">
                                <option value="">Selecione</option>
                                <option value="hemodialise">Hemodiálise</option>
                                <option value="dialise_peritoneal">Diálise Peritoneal</option>
                                <option value="transplante">Transplante Renal</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label for="tempoTRS" class="form-label">Tempo em TRS prévia (meses)</label>
                            <input type="number" id="tempoTRS" class="form-control" min="0">
                        </div>
                    </div>
                    
                    <div class="mt-4">
                        <button type="button" id="nextToLab" class="btn btn-primary">Próximo: Parâmetros Laboratoriais</button>
                    </div>
                </div>
            </div>
            
            <!-- Parâmetros Laboratoriais -->
            <div id="parametros-laboratoriais" class="tab-content">
                <div class="card">
                    <h2 class="text-xl font-semibold mb-4">Parâmetros Laboratoriais</h2>
                    
                    <div class="flex-container">
                        <div class="form-group">
                            <label for="ureia" class="form-label">Ureia (mg/dL)</label>
                            <input type="number" id="ureia" class="form-control" min="0" step="0.1">
                        </div>
                        <div class="form-group">
                            <label for="creatinina" class="form-label">Creatinina (mg/dL)</label>
                            <input type="number" id="creatinina" class="form-control" min="0" step="0.01">
                        </div>
                    </div>
                    
                    <div class="flex-container">
                        <div class="form-group">
                            <label for="potassio" class="form-label">Potássio (mEq/L)</label>
                            <input type="number" id="potassio" class="form-control" min="0" step="0.1">
                        </div>
                        <div class="form-group">
                            <label for="bicarbonato" class="form-label">Bicarbonato (mEq/L)</label>
                            <input type="number" id="bicarbonato" class="form-control" min="0" step="0.1">
                        </div>
                    </div>
                    
                    <div class="flex-container">
                        <div class="form-group">
                            <label for="ph" class="form-label">pH Arterial</label>
                            <input type="number" id="ph" class="form-control" min="6.5" max="8.0" step="0.01">
                        </div>
                        <div class="form-group">
                            <label for="fosforo" class="form-label">Fósforo (mg/dL)</label>
                            <input type="number" id="fosforo" class="form-control" min="0" step="0.1">
                        </div>
                    </div>
                    
                    <div class="flex-container">
                        <div class="form-group">
                            <label for="calcio" class="form-label">Cálcio total (mg/dL)</label>
                            <input type="number" id="calcio" class="form-control" min="0" step="0.1">
                        </div>
                        <div class="form-group">
                            <label for="albumina" class="form-label">Albumina (g/dL)</label>
                            <input type="number" id="albumina" class="form-control" min="0" step="0.1">
                        </div>
                    </div>
                    
                    <div class="flex-container">
                        <div class="form-group">
                            <label for="hemoglobina" class="form-label">Hemoglobina (g/dL)</label>
                            <input type="number" id="hemoglobina" class="form-control" min="0" step="0.1">
                        </div>
                        <div class="form-group">
                            <label for="tfg" class="form-label">TFG estimada (mL/min/1.73m²)</label>
                            <input type="number" id="tfg" class="form-control" min="0" step="0.1">
                        </div>
                    </div>
                    
                    <div class="alert alert-info mt-3">
                        <p class="text-sm">
                            Caso não possua o valor de TFG, o sistema calculará utilizando a fórmula CKD-EPI com os dados de creatinina, idade, sexo e raça.
                        </p>
                    </div>
                    
                    <div class="mt-4 flex-container">
                        <button type="button" id="backToPaciente" class="btn btn-danger">Anterior: Dados do Paciente</button>
                        <button type="button" id="nextToClinica" class="btn btn-primary">Próximo: Avaliação Clínica</button>
                    </div>
                </div>
            </div>
            
            <!-- Avaliação Clínica -->
            <div id="avaliacao-clinica" class="tab-content">
                <div class="card">
                    <h2 class="text-xl font-semibold mb-4">Avaliação Clínica</h2>
                    
                    <div class="flex-container">
                        <div class="form-group">
                            <label for="diurese" class="form-label">Diurese nas últimas 24h (mL)</label>
                            <input type="number" id="diurese" class="form-control" min="0">
                        </div>
                        <div class="form-group">
                            <label for="balancoHidrico" class="form-label">Balanço Hídrico (mL)</label>
                            <input type="number" id="balancoHidrico" class="form-control">
                            <p class="text-xs mt-1 opacity-75">Positivo para retenção, negativo para perda de volume</p>
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Estado Volêmico</label>
                        <div class="form-check">
                            <input type="radio" id="volemia_hipovolemico" name="estado_volemico" class="form-check-input" value="hipovolemico">
                            <label for="volemia_hipovolemico">Hipovolêmico</label>
                        </div>
                        <div class="form-check">
                            <input type="radio" id="volemia_euvolemico" name="estado_volemico" class="form-check-input" value="euvolemico">
                            <label for="volemia_euvolemico">Euvolêmico</label>
                        </div>
                        <div class="form-check">
                            <input type="radio" id="volemia_hipervolemico" name="estado_volemico" class="form-check-input" value="hipervolemico">
                            <label for="volemia_hipervolemico">Hipervolêmico</label>
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Sinais de Congestão</label>
                        <div class="form-check">
                            <input type="checkbox" id="edema_periférico" class="form-check-input" value="edema_periférico">
                            <label for="edema_periférico">Edema Periférico</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="anasarca" class="form-check-input" value="anasarca">
                            <label for="anasarca">Anasarca</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="ascite" class="form-check-input" value="ascite">
                            <label for="ascite">Ascite</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="derrame_pleural" class="form-check-input" value="derrame_pleural">
                            <label for="derrame_pleural">Derrame Pleural</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="congestao_pulmonar" class="form-check-input" value="congestao_pulmonar">
                            <label for="congestao_pulmonar">Congestão Pulmonar</label>
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label for="estadoNeurologico" class="form-label">Estado Neurológico</label>
                        <select id="estadoNeurologico" class="form-control">
                            <option value="">Selecione</option>
                            <option value="normal">Normal</option>
                            <option value="sonolencia">Sonolência</option>
                            <option value="confusao">Confusão Mental</option>
                            <option value="agitacao">Agitação Psicomotora</option>
                            <option value="coma">Coma</option>
                        </select>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Manifestações Urêmicas</label>
                        <div class="form-check">
                            <input type="checkbox" id="nauseas" class="form-check-input" value="nauseas">
                            <label for="nauseas">Náuseas/Vômitos</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="anorexia" class="form-check-input" value="anorexia">
                            <label for="anorexia">Anorexia</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="prurido" class="form-check-input" value="prurido">
                            <label for="prurido">Prurido</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="solucos" class="form-check-input" value="solucos">
                            <label for="solucos">Soluços</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="sangramento" class="form-check-input" value="sangramento">
                            <label for="sangramento">Sangramento</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="pericardite" class="form-check-input" value="pericardite">
                            <label for="pericardite">Pericardite</label>
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label for="pa" class="form-label">Pressão Arterial (mmHg)</label>
                        <div class="flex-container">
                            <input type="number" id="pas" class="form-control" placeholder="Sistólica" min="0">
                            <input type="number" id="pad" class="form-control" placeholder="Diastólica" min="0">
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label for="statusRespiratorio" class="form-label">Status Respiratório</label>
                        <select id="statusRespiratorio" class="form-control">
                            <option value="">Selecione</option>
                            <option value="normal">Normal (ar ambiente)</option>
                            <option value="suplementacao">Com suplementação de O2</option>
                            <option value="vni">Ventilação Não Invasiva</option>
                            <option value="vmi">Ventilação Mecânica Invasiva</option>
                        </select>
                    </div>
                    
                    <div class="form-group">
                        <label for="urgenciaDialise" class="form-label">Urgência para Diálise</label>
                        <input type="range" id="urgenciaDialise" min="1" max="5" step="1" value="3">
                        <div class="range-labels">
                            <span>Eletiva</span>
                            <span>Urgência relativa</span>
                            <span>Emergência</span>
                        </div>
                    </div>
                    
                    <div class="mt-4 flex-container">
                        <button type="button" id="backToLab" class="btn btn-danger">Anterior: Parâmetros Laboratoriais</button>
                        <button type="button" id="nextToComorbidades" class="btn btn-primary">Próximo: Comorbidades</button>
                    </div>
                </div>
            </div>
            
            <!-- Comorbidades -->
            <div id="comorbidades" class="tab-content">
                <div class="card">
                    <h2 class="text-xl font-semibold mb-4">Comorbidades</h2>
                    
                    <div class="form-group">
                        <label class="form-label">Doenças Cardiovasculares</label>
                        <div class="form-check">
                            <input type="checkbox" id="hipertensao" class="form-check-input" value="hipertensao">
                            <label for="hipertensao">Hipertensão Arterial</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="iccc" class="form-check-input" value="iccc">
                            <label for="iccc">Insuficiência Cardíaca Congestiva</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="coronariopatia" class="form-check-input" value="coronariopatia">
                            <label for="coronariopatia">Doença Arterial Coronariana</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="arritmia" class="form-check-input" value="arritmia">
                            <label for="arritmia">Arritmia Cardíaca</label>
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Doenças Metabólicas</label>
                        <div class="form-check">
                            <input type="checkbox" id="dm" class="form-check-input" value="dm">
                            <label for="dm">Diabetes Mellitus</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="dislipidemia" class="form-check-input" value="dislipidemia">
                            <label for="dislipidemia">Dislipidemia</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="obesidade" class="form-check-input" value="obesidade">
                            <label for="obesidade">Obesidade</label>
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Outras Comorbidades</label>
                        <div class="form-check">
                            <input type="checkbox" id="hepatopatia" class="form-check-input" value="hepatopatia">
                            <label for="hepatopatia">Hepatopatia</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="neoplasia" class="form-check-input" value="neoplasia">
                            <label for="neoplasia">Neoplasia</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="infeccao" class="form-check-input" value="infeccao">
                            <label for="infeccao">Infecção Ativa</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="doenca_autoimune" class="form-check-input" value="doenca_autoimune">
                            <label for="doenca_autoimune">Doença Autoimune</label>
                        </div>
                        <div class="form-check">
                            <input type="checkbox" id="avc" class="form-check-input" value="avc">
                            <label for="avc">AVC Prévio</label>
                        </div>
                    </div>
                    
                    <div class="form-group">
                        <label for="medicamentosAtuais" class="form-label">Medicamentos em Uso Atual</label>
                        <textarea id="medicamentosAtuais" class="form-control" rows="3" placeholder="Liste os medicamentos atuais do paciente"></textarea>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Contraindicações para Heparina</label>
                        <div class="form-check">
                            <input type="radio" id="heparina_sim" name="contraindicacao_heparina" class="form-check-input" value="sim">
                            <label for="heparina_sim">Sim</label>
                        </div>
                        <div class="form-check">
                            <input type="radio" id="heparina_nao" name="contraindicacao_heparina" class="form-check-input" value="nao">
                            <label for="heparina_nao">Não</label>
                        </div>
                    </div>
                    
                    <div id="contraindicacaoHeparinaDetails" class="form-group hidden">
                        <label for="motivoContraindicacaoHeparina" class="form-label">Motivo da Contraindicação</label>
                        <select id="motivoContraindicacaoHeparina" class="form-control">
                            <option value="">Selecione</option>
                            <option value="sangramento_ativo">Sangramento Ativo</option>
                            <option value="sangramento_recente">Sangramento Recente (< 48h)</option>
                            <option value="plaquetopenia">Plaquetopenia Grave</option>
                            <option value="pos_cirurgia">Pós-Operatório Imediato</option>
                            <option value="hit">Trombocitopenia induzida por heparina</option>
                            <option value="outro">Outro</option>
                        </select>
                    </div>
                    
                    <div class="form-group">
                        <label for="observacoes" class="form-label">Observações Adicionais</label>
                        <textarea id="observacoes" class="form-control" rows="3" placeholder="Observações importantes para decisão terapêutica"></textarea>
                    </div>
                    
                    <div class="mt-4 flex-container">
                        <button type="button" id="backToClinica" class="btn btn-danger">Anterior: Avaliação Clínica</button>
                        <button type="button" id="calcularResultado" class="btn btn-success">Calcular Resultado</button>
                    </div>
                </div>
            </div>
            
            <!-- Resultado -->
            <div id="resultado" class="tab-content">
                <div class="card">
                    <h2 class="text-xl font-semibold mb-4">Resultado da Avaliação</h2>
                    
                    <div id="loadingResult" class="spinner"></div>
                    
                    <div id="resultContent" class="hidden">
                        <!-- Será preenchido dinamicamente -->
                    </div>
                    
                    <div id="prescricaoContainer" class="hidden">
                        <h3 class="text-lg font-semibold mt-6 mb-3">Sugestão de Prescrição</h3>
                        <div id="prescricaoContent">
                            <!-- Será preenchido dinamicamente -->
                        </div>
                    </div>
                    
                    <div class="mt-6">
                        <p class="text-sm opacity-75">Este é um assistente de decisão e não substitui o julgamento clínico profissional. A decisão final sobre indicação e prescrição de diálise deve considerar todos os aspectos clínicos individuais do paciente.</p>
                    </div>
                    
                    <div class="mt-4 flex-container">
                        <button type="button" id="backToComorbidades" class="btn btn-danger">Anterior: Comorbidades</button>
                        <button type="button" id="novaPesquisa" class="btn btn-primary">Nova Pesquisa</button>
                    </div>
                </div>
            </div>
        </form>
    </div>
    
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Elementos das abas
            const tabs = document.querySelectorAll('.tab');
            const tabContents = document.querySelectorAll('.tab-content');
            
            // Função para mostrar uma aba específica
            function showTab(tabId) {
                // Esconder todas as abas
                tabContents.forEach(content => {
                    content.classList.remove('active');
                });
                
                tabs.forEach(tab => {
                    tab.classList.remove('active');
                });
                
                // Mostrar a aba selecionada
                document.getElementById(tabId).classList.add('active');
                document.querySelector(`.tab[data-tab="${tabId}"]`).classList.add('active');
                
                // Scroll para o topo
                window.scrollTo(0, 0);
            }
            
            // Configurar navegação entre abas
            tabs.forEach(tab => {
                tab.addEventListener('click', function() {
                    const tabId = this.getAttribute('data-tab');
                    showTab(tabId);
                });
            });
            
            // Botões de navegação
            document.getElementById('nextToLab').addEventListener('click', () => showTab('parametros-laboratoriais'));
            document.getElementById('backToPaciente').addEventListener('click', () => showTab('dados-paciente'));
            document.getElementById('nextToClinica').addEventListener('click', () => showTab('avaliacao-clinica'));
            document.getElementById('backToLab').addEventListener('click', () => showTab('parametros-laboratoriais'));
            document.getElementById('nextToComorbidades').addEventListener('click', () => showTab('comorbidades'));
            document.getElementById('backToClinica').addEventListener('click', () => showTab('avaliacao-clinica'));
            
            document.getElementById('calcularResultado').addEventListener('click', () => {
                showTab('resultado');
                avaliarNecessidadeDialise();
            });
            
            document.getElementById('backToComorbidades').addEventListener('click', () => showTab('comorbidades'));
            document.getElementById('novaPesquisa').addEventListener('click', resetForm);
            
            // Toggle para mostrar/esconder campos condicionais
            document.querySelectorAll('input[name="historico_trs"]').forEach(radio => {
                radio.addEventListener('change', function() {
                    document.getElementById('historicoTRSDetails').classList.toggle('hidden', this.value !== 'sim');
                });
            });
            
            document.querySelectorAll('input[name="contraindicacao_heparina"]').forEach(radio => {
                radio.addEventListener('change', function() {
                    document.getElementById('contraindicacaoHeparinaDetails').classList.toggle('hidden', this.value !== 'sim');
                });
            });
            
            // Função para calcular TFG usando CKD-EPI
            function calcularTFG(creatinina, idade, sexo, raca) {
                creatinina = parseFloat(creatinina);
                idade = parseInt(idade);
                
                if (isNaN(creatinina) || isNaN(idade)) {
                    return null;
                }
                
                // Converter creatinina de mg/dL para μmol/L (fator de multiplicação: 88.4)
                const creatininaMicromol = creatinina * 88.4;
                
                let k, alpha;
                if (sexo === 'F') {
                    k = 0.7;
                    alpha = -0.329;
                } else {
                    k = 0.9;
                    alpha = -0.411;
                }
                
                let racaFator = raca === 'negra' ? 1.159 : 1.0;
                
                // Fórmula CKD-EPI
                let parteA = Math.min(creatinina / k, 1) ** alpha;
                let parteB = Math.max(creatinina / k, 1) ** -1.209;
                let tfg = 141 * Math.min(parteA, parteB) * Math.pow(0.993, idade);
                
                if (sexo === 'F') {
                    tfg *= 1.018;
                }
                
                tfg *= racaFator;
                
                return tfg.toFixed(2);
            }
            
            // Função para avaliar a necessidade de diálise
            function avaliarNecessidadeDialise() {
                // Mostrar spinner de carregamento
                document.getElementById('loadingResult').classList.remove('hidden');
                document.getElementById('resultContent').classList.add('hidden');
                document.getElementById('prescricaoContainer').classList.add('hidden');
                
                // Simular processamento (na prática, é instantâneo, mas melhora a experiência do usuário)
                setTimeout(() => {
                    // Coletar dados do formulário
                    const dados = {
                        // Dados do paciente
                        nome: document.getElementById('nome').value,
                        idade: document.getElementById('idade').value,
                        sexo: document.getElementById('sexo').value,
                        peso: parseFloat(document.getElementById('peso').value) || 0,
                        pesoSeco: parseFloat(document.getElementById('pesoSeco').value) || 0,
                        acessoVascular: document.getElementById('acessoVascular').value,
                        
                        // Parâmetros laboratoriais
                        ureia: parseFloat(document.getElementById('ureia').value) || 0,
                        creatinina: parseFloat(document.getElementById('creatinina').value) || 0,
                        potassio: parseFloat(document.getElementById('potassio').value) || 0,
                        bicarbonato: parseFloat(document.getElementById('bicarbonato').value) || 0,
                        ph: parseFloat(document.getElementById('ph').value) || 0,
                        tfg: parseFloat(document.getElementById('tfg').value) || 0,
                        
                        // Se TFG não estiver preenchida, calcular
                        calcularTFG: function() {
                            if (this.tfg <= 0 && this.creatinina > 0 && this.idade > 0 && this.sexo) {
                                // Consideramos raça como não-negra por padrão
                                this.tfg = calcularTFG(this.creatinina, this.idade, this.sexo, 'nao-negra');
                                return this.tfg;
                            }
                            return this.tfg;
                        },
                        
                        // Avaliação clínica
                        diurese: parseFloat(document.getElementById('diurese').value) || 0,
                        balancoHidrico: parseFloat(document.getElementById('balancoHidrico').value) || 0,
                        estadoVolemico: document.querySelector('input[name="estado_volemico"]:checked')?.value || '',
                        estadoNeurologico: document.getElementById('estadoNeurologico').value,
                        pas: parseFloat(document.getElementById('pas').value) || 0,
                        pad: parseFloat(document.getElementById('pad').value) || 0,
                        statusRespiratorio: document.getElementById('statusRespiratorio').value,
                        urgenciaDialise: parseInt(document.getElementById('urgenciaDialise').value) || 3,
                        
                        // Verificar sinais de congestão
                        sinaisCongestao: function() {
                            const sinais = [];
                            document.querySelectorAll('input[type="checkbox"]:checked').forEach(checkbox => {
                                if (['edema_periférico', 'anasarca', 'ascite', 'derrame_pleural', 'congestao_pulmonar'].includes(checkbox.value)) {
                                    sinais.push(checkbox.value);
                                }
                            });
                            return sinais;
                        },
                        
                        // Verificar manifestações urêmicas
                        manifestacoesUremicas: function() {
                            const manifestacoes = [];
                            document.querySelectorAll('input[type="checkbox"]:checked').forEach(checkbox => {
                                if (['nauseas', 'anorexia', 'prurido', 'solucos', 'sangramento', 'pericardite'].includes(checkbox.value)) {
                                    manifestacoes.push(checkbox.value);
                                }
                            });
                            return manifestacoes;
                        },
                        
                        // Contraindicações para heparina
                        contraindicacaoHeparina: document.querySelector('input[name="contraindicacao_heparina"]:checked')?.value === 'sim',
                        motivoContraindicacaoHeparina: document.getElementById('motivoContraindicacaoHeparina').value,
                    };
                    
                    // Calcular TFG se necessário
                    dados.calcularTFG();
                    
                    // Avaliar indicação de diálise
                    const resultado = avaliarIndicacaoDialise(dados);
                    
                    // Esconder spinner e mostrar resultado
                    document.getElementById('loadingResult').classList.add('hidden');
                    document.getElementById('resultContent').classList.remove('hidden');
                    
                    // Preencher conteúdo do resultado
                    document.getElementById('resultContent').innerHTML = gerarHTMLResultado(resultado);
                    
                    // Se houver indicação de diálise, mostrar sugestão de prescrição
                    if (resultado.indicacao === 'emergencia' || resultado.indicacao === 'urgencia' || 
                        resultado.indicacao === 'eletiva') {
                        const prescricao = gerarPrescricaoDialise(dados, resultado);
                        document.getElementById('prescricaoContainer').classList.remove('hidden');
                        document.getElementById('prescricaoContent').innerHTML = gerarHTMLPrescricao(prescricao);
                    }
                }, 1000);
            }
            
            // Função para avaliar indicação de diálise baseada nos dados
            function avaliarIndicacaoDialise(dados) {
                let indicacao = 'sem_indicacao'; // Valores possíveis: emergencia, urgencia, eletiva, sem_indicacao, considerar
                let motivos = [];
                let criteriosEmergencia = false;
                let criteriosUrgencia = false;
                let criteriosEletivos = false;
                
                // 1. Critérios para diálise de Emergência (absolutos)
                
                // Hipercalemia severa (K > 6.5)
                if (dados.potassio > 6.5) {
                    motivos.push('Hipercalemia severa (K > 6.5 mEq/L)');
                    criteriosEmergencia = true;
                }
                
                // Acidose metabólica grave (pH < 7.1 ou Bicarbonato < 10)
                if (dados.ph < 7.1) {
                    motivos.push('Acidose metabólica grave (pH < 7.1)');
                    criteriosEmergencia = true;
                }
                
                if (dados.bicarbonato < 10) {
                    motivos.push('Acidose metabólica grave (Bicarbonato < 10 mEq/L)');
                    criteriosEmergencia = true;
                }
                
                // Uremia severa sintomática (Ureia > 150 + sintomas neurológicos)
                if (dados.ureia > 150 && ['confusao', 'agitacao', 'coma'].includes(dados.estadoNeurologico)) {
                    motivos.push('Uremia severa sintomática (Ureia > 150 mg/dL com alteração neurológica)');
                    criteriosEmergencia = true;
                }
                
                // Congestão pulmonar/edema agudo não responsivo a diuréticos
                if (dados.sinaisCongestao().includes('congestao_pulmonar') && 
                    dados.estadoVolemico === 'hipervolemico' && 
                    dados.diurese < 500) {
                    motivos.push('Congestão pulmonar com oligúria não responsiva a diuréticos');
                    criteriosEmergencia = true;
                }
                
                // Pericardite urêmica
                if (dados.manifestacoesUremicas().includes('pericardite')) {
                    motivos.push('Pericardite urêmica');
                    criteriosEmergencia = true;
                }
                
                // 2. Critérios para diálise de Urgência (relativos)
                
                // Hipercalemia moderada (K entre 6.0 e 6.5)
                if (dados.potassio >= 6.0 && dados.potassio <= 6.5) {
                    motivos.push('Hipercalemia moderada (K entre 6.0 e 6.5 mEq/L)');
                    criteriosUrgencia = true;
                }
                
                // Acidose metabólica moderada (pH entre 7.1 e 7.2 ou Bicarbonato entre 10 e 15)
                if ((dados.ph >= 7.1 && dados.ph < 7.2) || (dados.bicarbonato >= 10 && dados.bicarbonato < 15)) {
                    motivos.push('Acidose metabólica moderada');
                    criteriosUrgencia = true;
                }
                
                // Uremia moderada (Ureia entre 100 e 150) + sintomas
                if (dados.ureia >= 100 && dados.ureia <= 150 && 
                    dados.manifestacoesUremicas().length >= 2) {
                    motivos.push('Uremia moderada com sintomas');
                    criteriosUrgencia = true;
                }
                
                // Sobrecarga hídrica sintomática
                if (dados.estadoVolemico === 'hipervolemico' && 
                    dados.sinaisCongestao().length >= 2) {
                    motivos.push('Sobrecarga hídrica sintomática');
                    criteriosUrgencia = true;
                }
                
                // 3. Critérios para diálise Eletiva
                
                // Doença renal crônica avançada (TFG < 10)
                if (dados.tfg < 10) {
                    motivos.push('Doença renal crônica avançada (TFG < 10 mL/min/1.73m²)');
                    criteriosEletivos = true;
                }
                
                // Uremia leve (Ureia entre 80 e 100) + sintomas
                if (dados.ureia >= 80 && dados.ureia < 100 && 
                    dados.manifestacoesUremicas().length >= 1) {
                    motivos.push('Uremia leve com sintomas');
                    criteriosEletivos = true;
                }
                
                // Balanço hídrico persistentemente positivo
                if (dados.estadoVolemico === 'hipervolemico' && 
                    dados.balancoHidrico > 2000) {
                    motivos.push('Balanço hídrico persistentemente positivo');
                    criteriosEletivos = true;
                }
                
                // 4. Determinar nível de indicação final
                if (criteriosEmergencia) {
                    indicacao = 'emergencia';
                } else if (criteriosUrgencia) {
                    indicacao = 'urgencia';
                } else if (criteriosEletivos) {
                    indicacao = 'eletiva';
                } else if (dados.tfg < 15 || dados.ureia > 70 || dados.creatinina > 6) {
                    indicacao = 'considerar';
                    motivos.push('Valores laboratoriais alterados, mas sem critérios definitivos para diálise');
                }
                
                // Verificar informações adicionais para a decisão
                const adicionais = [];
                
                // Avaliação se o paciente está oligúrico
                if (dados.diurese < 400) {
                    adicionais.push('Paciente oligúrico (< 400 mL/dia)');
                }
                
                // Avaliação da função renal
                if (dados.tfg >= 10 && dados.tfg < 15) {
                    adicionais.push('TFG entre 10-15 mL/min/1.73m²: considerar início em condições específicas');
                }
                
                // Avaliar acesso vascular
                if (!dados.acessoVascular || dados.acessoVascular === 'sem_acesso') {
                    adicionais.push('Paciente sem acesso vascular para diálise');
                }
                
                return {
                    indicacao: indicacao,
                    motivos: motivos,
                    adicionais: adicionais,
                    nivel_urgencia: dados.urgenciaDialise
                };
            }
            
            // Função para gerar HTML do resultado
            function gerarHTMLResultado(resultado) {
                let html = '';
                let cardClass = '';
                let textoIndicacao = '';
                
                switch (resultado.indicacao) {
                    case 'emergencia':
                        cardClass = 'danger';
                        textoIndicacao = 'Diálise de Emergência';
                        break;
                    case 'urgencia':
                        cardClass = 'warning';
                        textoIndicacao = 'Diálise de Urgência';
                        break;
                    case 'eletiva':
                        cardClass = 'success';
                        textoIndicacao = 'Diálise Eletiva';
                        break;
                    case 'considerar':
                        cardClass = 'primary';
                        textoIndicacao = 'Considerar Diálise';
                        break;
                    default:
                        textoIndicacao = 'Sem Indicação de Diálise no Momento';
                }
                
                html += `<div class="result-card ${cardClass}">`;
                html += `<h3>${textoIndicacao}</h3>`;
                
                // Adicionar motivos
                if (resultado.motivos.length > 0) {
                    html += `<p class="font-semibold mt-3 mb-2">Motivos para indicação:</p>`;
                    html += `<ul class="list-disc pl-5">`;
                    resultado.motivos.forEach(motivo => {
                        html += `<li>${motivo}</li>`;
                    });
                    html += `</ul>`;
                }
                
                // Adicionar informações adicionais
                if (resultado.adicionais.length > 0) {
                    html += `<p class="font-semibold mt-3 mb-2">Considerações adicionais:</p>`;
                    html += `<ul class="list-disc pl-5">`;
                    resultado.adicionais.forEach(adicional => {
                        html += `<li>${adicional}</li>`;
                    });
                    html += `</ul>`;
                }
                
                // Nível de urgência
                let urgenciaTexto = '';
                switch (resultado.nivel_urgencia) {
                    case 1:
                        urgenciaTexto = 'Eletiva - pode ser programada';
                        break;
                    case 2:
                        urgenciaTexto = 'Eletiva com prioridade';
                        break;
                    case 3:
                        urgenciaTexto = 'Urgência relativa - nas próximas horas';
                        break;
                    case 4:
                        urgenciaTexto = 'Urgência - diálise imediata';
                        break;
                    case 5:
                        urgenciaTexto = 'Emergência - diálise imediata';
                        break;
                }
                
                html += `<p class="mt-3"><strong>Nível de urgência avaliado:</strong> ${urgenciaTexto}</p>`;
                
                html += `</div>`;
                
                return html;
            }
            
            // Função para gerar prescrição de diálise
            function gerarPrescricaoDialise(dados, resultado) {
                const prescricao = {
                    tipo: 'hemodialise', // Padrão é hemodiálise
                    duracao: 3, // Em horas
                    fluxoSangue: 0,
                    fluxoDialisato: 500, // mL/min
                    ultrafiltracaoAlvo: 0, // mL
                    anticoagulacao: dados.contraindicacaoHeparina ? 'sem_anticoagulacao' : 'heparina',
                    doseHeparina: 0,
                    solucaoDialise: 'padrao', // padrao, alto_k, baixo_k, alto_ca, baixo_ca
                    bicarbonato: 32, // mEq/L
                    observacoes: []
                };
                
                // Definir tipo de diálise baseado na urgência
                if (resultado.indicacao === 'emergencia') {
                    prescricao.duracao = 2; // Sessões de emergência costumam ser mais curtas
                } else if (resultado.indicacao === 'urgencia') {
                    prescricao.duracao = 3;
                } else {
                    prescricao.duracao = 4;
                }
                
                // Calcular fluxo sanguíneo baseado no peso
                if (dados.peso > 0) {
                    prescricao.fluxoSangue = Math.min(Math.round(dados.peso * 3), 350);
                    // Limitar a 300 para diálises de emergência/urgência
                    if (resultado.indicacao === 'emergencia' || resultado.indicacao === 'urgencia') {
                        prescricao.fluxoSangue = Math.min(prescricao.fluxoSangue, 300);
                    }
                } else {
                    prescricao.fluxoSangue = 250; // Valor padrão se peso não informado
                }
                
                // Calcular ultrafiltração alvo baseado no estado volêmico
                if (dados.estadoVolemico === 'hipervolemico') {
                    if (dados.peso > 0 && dados.pesoSeco > 0) {
                        const excessoLiquido = dados.peso - dados.pesoSeco;
                        // Limitar UF a 10 mL/kg/h
                        const ufMaxima = dados.peso * 10 * prescricao.duracao;
                        prescricao.ultrafiltracaoAlvo = Math.min(excessoLiquido * 1000, ufMaxima);
                        prescricao.ultrafiltracaoAlvo = Math.round(prescricao.ultrafiltracaoAlvo / 100) * 100; // Arredondar para centenas
                    } else if (dados.balancoHidrico > 1000) {
                        prescricao.ultrafiltracaoAlvo = Math.min(dados.balancoHidrico, 3000);
                    } else {
                        prescricao.ultrafiltracaoAlvo = 2000; // Valor padrão para sobrecarga hídrica
                    }
                } else if (dados.estadoVolemico === 'euvolemico') {
                    prescricao.ultrafiltracaoAlvo = 500; // UF mínima para euvolêmicos
                } else {
                    prescricao.ultrafiltracaoAlvo = 0; // Sem UF para hipovolêmicos
                }
                
                // Ajustar solução de diálise baseado nos valores laboratoriais
                if (dados.potassio > 6.0) {
                    prescricao.solucaoDialise = 'baixo_k';
                    prescricao.observacoes.push('Utilizar solução com baixo potássio devido à hipercalemia');
                } else if (dados.potassio < 3.5) {
                    prescricao.solucaoDialise = 'alto_k';
                    prescricao.observacoes.push('Utilizar solução com alto potássio para evitar hipocalemia');
                }
                
                // Ajustar bicarbonato baseado na acidose
                if (dados.bicarbonato < 15 || dados.ph < 7.2) {
                    prescricao.bicarbonato = 35;
                    prescricao.observacoes.push('Aumentado bicarbonato do banho devido à acidose metabólica');
                }
                
                // Definir anticoagulação
                if (prescricao.anticoagulacao === 'heparina') {
                    // Dose padrão de heparina
                    if (dados.peso > 0) {
                        prescricao.doseHeparina = Math.round(dados.peso * 50 / 100) * 100; // Arredondar para centenas
                    } else {
                        prescricao.doseHeparina = 3000; // Valor padrão
                    }
                } else {
                    prescricao.observacoes.push(`Contraindicação para heparina: ${dados.motivoContraindicacaoHeparina || 'Não especificado'}`);
                    prescricao.observacoes.push('Considerar lavagem do sistema com SF 0,9% a cada 30 minutos');
                }
                
                // Considerações sobre o acesso vascular
                if (dados.acessoVascular === 'cateter_temporario' || dados.acessoVascular === 'cateter_permanente') {
                    // Reduzir fluxo sanguíneo para cateteres temporários
                    if (dados.acessoVascular === 'cateter_temporario') {
                        prescricao.fluxoSangue = Math.min(prescricao.fluxoSangue, 250);
                        prescricao.observacoes.push('Fluxo sanguíneo limitado devido ao uso de cateter temporário');
                    }
                } else if (dados.acessoVascular === 'sem_acesso') {
                    prescricao.observacoes.push('ATENÇÃO: Paciente necessita de confecção de acesso vascular antes da diálise');
                }
                
                return prescricao;
            }
            
            // Função para gerar HTML da prescrição de diálise
            function gerarHTMLPrescricao(prescricao) {
                let html = `<div class="prescription-section">`;
                
                // Tabela com os parâmetros da prescrição
                html += `<table>
                    <tr>
                        <th colspan="2">Parâmetros da Diálise</th>
                    </tr>
                    <tr>
                        <td>Modalidade</td>
                        <td>Hemodiálise Convencional</td>
                    </tr>
                    <tr>
                        <td>Duração</td>
                        <td>${prescricao.duracao} horas</td>
                    </tr>
                    <tr>
                        <td>Fluxo Sanguíneo (Qb)</td>
                        <td>${prescricao.fluxoSangue} mL/min</td>
                    </tr>
                    <tr>
                        <td>Fluxo Dialisato (Qd)</td>
                        <td>${prescricao.fluxoDialisato} mL/min</td>
                    </tr>
                    <tr>
                        <td>Ultrafiltração Alvo</td>
                        <td>${prescricao.ultrafiltracaoAlvo} mL</td>
                    </tr>`;
                
                // Anticoagulação
                html += `<tr>
                    <td>Anticoagulação</td>
                    <td>`;
                
                if (prescricao.anticoagulacao === 'heparina') {
                    html += `Heparina: ${prescricao.doseHeparina} UI (bólus inicial de ${Math.round(prescricao.doseHeparina/3)} UI)`;
                } else {
                    html += `Sem anticoagulação`;
                }
                
                html += `</td></tr>`;
                
                // Solução de diálise
                html += `<tr>
                    <td>Solução de Diálise</td>
                    <td>`;
                
                switch (prescricao.solucaoDialise) {
                    case 'baixo_k':
                        html += `Solução com baixo potássio (1.0 mEq/L)`;
                        break;
                    case 'alto_k':
                        html += `Solução com alto potássio (3.0 mEq/L)`;
                        break;
                    case 'alto_ca':
                        html += `Solução com alto cálcio (3.5 mEq/L)`;
                        break;
                    case 'baixo_ca':
                        html += `Solução com baixo cálcio (2.5 mEq/L)`;
                        break;
                    default:
                        html += `Solução padrão (K: 2.0 mEq/L, Ca: 3.0 mEq/L)`;
                }
                
                html += `</td></tr>`;
                
                html += `<tr>
                    <td>Bicarbonato</td>
                    <td>${prescricao.bicarbonato} mEq/L</td>
                </tr>`;
                
                html += `</table>`;
                
                // Observações e recomendações adicionais
                if (prescricao.observacoes.length > 0) {
                    html += `<div class="mt-4">
                        <p class="font-semibold">Observações e Recomendações:</p>
                        <ul class="list-disc pl-5 mt-2">`;
                    
                    prescricao.observacoes.forEach(obs => {
                        html += `<li>${obs}</li>`;
                    });
                    
                    html += `</ul>
                    </div>`;
                }
                
                html += `</div>`;
                
                return html;
            }
            
            // Função para resetar o formulário
            function resetForm() {
                document.getElementById('dialysisForm').reset();
                showTab('dados-paciente');
                document.getElementById('resultContent').innerHTML = '';
                document.getElementById('prescricaoContent').innerHTML = '';
                document.getElementById('contraindicacaoHeparinaDetails').classList.add('hidden');
                document.getElementById('historicoTRSDetails').classList.add('hidden');
            }
        });
    </script>
</body>
</html>
