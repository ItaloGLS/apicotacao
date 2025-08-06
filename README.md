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

    O app consulta a API economia.awesomeapi.com.br e busca a cotação atual da moeda selecionada em relação ao Real

    Exibe a alta, baixa, valor atual e data da última atualização da moeda

Estrutura do projeto

    MainActivity.kt: Lógica principal da tela de busca e exibição da cotação

    MoedaApi.kt: Interface Retrofit para definir as chamadas HTTP à API

    MoedaResponse.kt: Data class para modelar os dados recebidos da API

    activity_main.xml: Layout da tela principal com campos de entrada e botão

## 📂 Arquivos principais

### ✅ `MainActivity.kt`

```kotlin
package com.example.cotacaomoedas

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.*
import retrofit2.*
import retrofit2.converter.gson.GsonConverterFactory

class MainActivity : AppCompatActivity() {

    // Variáveis para referenciar os elementos da interface
    private lateinit var editMoeda: EditText
    private lateinit var btnBuscar: Button
    private lateinit var textResultado: TextView

    // Método chamado ao criar a Activity
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Define o layout da tela a ser usado
        setContentView(R.layout.activity_main)

        // Associa as variáveis com os componentes visuais do layout pelo ID
        editMoeda = findViewById(R.id.editMoeda)
        btnBuscar = findViewById(R.id.btnBuscar)
        textResultado = findViewById(R.id.textResultado)

        // Configuração do Retrofit para fazer requisições HTTP
        val retrofit = Retrofit.Builder()
            .baseUrl("https://economia.awesomeapi.com.br/")  // URL base da API
            .addConverterFactory(GsonConverterFactory.create())  // Conversor JSON para objetos Kotlin
            .build()

        // Cria uma instância da interface de chamadas da API
        val api = retrofit.create(MoedaApi::class.java)

        // Define o que acontece quando o botão é clicado
        btnBuscar.setOnClickListener {
            // Pega o texto digitado, remove espaços e converte para maiúscula
            val sigla = editMoeda.text.toString().trim().uppercase()

            // Se o campo estiver vazio, avisa o usuário
            if (sigla.isEmpty()) {
                textResultado.text = "Digite a sigla da moeda!"
                return@setOnClickListener
            }

            // Faz a chamada para buscar a cotação da moeda
            val call = api.getCotacao(sigla)

            // Executa a requisição assincronamente
            call.enqueue(object : Callback<Map<String, MoedaResponse>> {
                // Se a resposta da API for recebida
                override fun onResponse(
                    call: Call<Map<String, MoedaResponse>>,
                    response: Response<Map<String, MoedaResponse>>
                ) {
                    if (response.isSuccessful) {  // Se status HTTP for 200
                        val body = response.body()
                        // A API retorna um mapa com chave "SIGLABRL", por exemplo "USDBRL"
                        val key = "${sigla}BRL"

                        val moeda = body?.get(key)
                        if (moeda != null) {
                            // Prepara o texto com as informações da moeda para mostrar ao usuário
                            val texto = """
                                Moeda: ${moeda.name}
                                Alta: R$ ${moeda.high}
                                Baixa: R$ ${moeda.low}
                                Valor atual: R$ ${moeda.bid}
                                Atualizado em: ${moeda.create_date}
                            """.trimIndent()
                            textResultado.text = texto
                        } else {
                            // Caso não encontre a moeda no mapa retornado
                            textResultado.text = "Moeda não encontrada."
                        }
                    } else {
                        // Caso a resposta HTTP não seja sucesso (ex: 404)
                        textResultado.text = "Erro na resposta da API."
                    }
                }

                // Caso a requisição falhe (ex: sem internet)
                override fun onFailure(call: Call<Map<String, MoedaResponse>>, t: Throwable) {
                    textResultado.text = "Erro: ${t.message}"
                }
            })
        }
    }
}
```
### ✅ `MoedaApi.kt`
```kotlin
package com.example.cotacaomoedas

import retrofit2.Call
import retrofit2.http.GET
import retrofit2.http.Path

interface MoedaApi {

    // Define o endpoint GET para buscar a cotação de uma moeda específica em relação ao real (BRL)
    // {moeda} será substituído pela sigla da moeda (ex: USD)
    // A resposta é um mapa onde a chave é a concatenação da moeda com BRL
    @GET("json/last/{moeda}-BRL")
    fun getCotacao(@Path("moeda") moeda: String): Call<Map<String, MoedaResponse>>
}
```
### ✅ `MoedaResponse.kt`
```kotlin
package com.example.cotacaomoedas

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
