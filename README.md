import androidx.compose.runtime.mutableStateOf
import androidx.lifecycle.ViewModel
import java.time.LocalDate
import java.time.temporal.ChronoUnit

data class MedicalEvolutionState(
    // Etapa 1: Identificação
    val nomePaciente: String = "",
    val leito: String = "",
    val idade: String = "",
    val dataInternacao: LocalDate = LocalDate.now(),
    
    // Etapa 2: Situação (POMR)
    val listaProblemas: String = "",
    
    // Etapa 3: Contexto
    val motivoInternacao: String = "",
    val hda: String = "",
    val situacaoAdmissional: String = "Avalio paciente no leito de enfermaria compartilhada, desacompanhado. Paciente consciente, orientado, respondendo a questionamentos sem alterações. Estável hemodinamicamente, boa perfusão. Eupneico em ar ambiente com bom padrão respiratório e sem sinais de desconforto respiratório. Diurese preservada com bom débito referido. Evacuações fisiológicas. Alimentando-se via oral sem queixas. Sono preservado. Nega queixas.",
    
    // Etapa 4: Histórico
    val comorbidades: String = "",
    val historiaSocial: String = "",
    val historiaCirurgica: String = "",
    val medicacoesUsoContinuo: String = "",
    val alergias: String = "",
    
    // Etapa 5: Avaliação Diária
    val evolucaoDiaria: String = "Avalio paciente no leito de enfermaria compartilhada, desacompanhado. Paciente consciente, orientado, respondendo a questionamentos sem alterações. Estável hemodinamicamente, boa perfusão. Eupneico em ar ambiente com bom padrão respiratório e sem sinais de desconforto respiratório. Diurese preservada com bom débito referido. Evacuações fisiológicas. Alimentando-se via oral sem queixas. Sono preservado. Nega queixas. Nas últimas 24h houve estabilidade do quadro que motivou internação.",
    val emUsoAntibiotico: Boolean = false,
    val atbNome: String = "",
    val atbDataInicio: String = "",
    val atbDataFim: String = "",
    
    // Sinais Vitais
    val fc: String = "", val fr: String = "", val paSistolica: String = "", 
    val paDiastolica: String = "", val tax: String = "", val spo2: String = "",
    val du: String = "", val gli: String = "",
    
    val exameFisico: String = "Bom estado geral, consciente e orientado no tempo e espaço, eupneico, acianótico, anictérico. AC: Ritmo cardíaco regular, em 2 tempos, bulhas normofonéticas, sem sopros. AR: Murmúrio vesicular universalmente audível, sem ruídos adventícios. Abdome: atípico, flácido, indolor, ruídos hidroaéreos presentes, sem megalias ou massas. Extremidades: bem perfundidas, sem edemas, pulsos palpáveis e simétricos.",
    val labs: String = "",
    val imagem: String = "",
    
    // Etapa 6: Condutas
    val condutas: String = "Conforme discutido com preceptoria: ",
    val planoTerapeutico: String = "",
    val infoRelevantes: String = ""
)

class EvolutionViewModel : ViewModel() {
    var state = mutableStateOf(MedicalEvolutionState())
        private set

    fun updateState(newState: MedicalEvolutionState) {
        state.value = newState
    }

    fun calcularDiasInternacao(): Long {
        return ChronoUnit.DAYS.between(state.value.dataInternacao, LocalDate.now())
    }

    fun gerarTextoFinal(): String {
        val s = state.value
        val dias = calcularDiasInternacao()
        
        return """
            EVOLUÇÃO MÉDICA - ${LocalDate.now()}
            
            IDENTIFICAÇÃO:
            Paciente: ${s.nomePaciente} | Leito: ${s.leito} | Idade: ${s.idade}
            Tempo de Internação: $dias dia(s)
            
            1. LISTA DE PROBLEMAS (POMR):
            ${s.listaProblemas}
            
            2. CONTEXTO E HISTÓRICO:
            Motivo: ${s.motivoInternacao}
            HDA: ${s.hda}
            Situação Admissional: ${s.situacaoAdmissional}
            Antecedentes: ${s.comorbidades} | Cirurgias: ${s.historiaCirurgica}
            Uso Contínuo: ${s.medicacoesUsoContinuo} | Alergias: ${s.alergias}
            
            3. AVALIAÇÃO DO DIA:
            ${s.evolucaoDiaria}
            ${if (s.emUsoAntibiotico) "ATB: ${s.atbNome} (Início: ${s.atbDataInicio} - Fim: ${s.atbDataFim})" else "Sem antibióticos no momento."}
            
            SINAIS VITAIS:
            PA: ${s.paSistolica}/${s.paDiastolica} mmHg | FC: ${s.fc} bpm | FR: ${s.fr} irpm
            Tax: ${s.tax} ºC | SpO2: ${s.spo2}% | DU: ${s.du} ml | Glic: ${s.gli} mg/dL
            
            EXAME FÍSICO:
            ${s.exameFisico}
            
            EXAMES COMPLEMENTARES:
            Labs: ${s.labs}
            Imagem: ${s.imagem}
            
            4. CONDUTA E PLANO (POR PROBLEMA):
            ${s.condutas}
            Plano: ${s.planoTerapeutico}
            Infos: ${s.infoRelevantes}
        """.trimIndent()
    }
}