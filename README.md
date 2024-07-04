package com.example.jogodavelha

import android.content.Intent
import android.graphics.Color
import android.os.Bundle
import android.view.View
import android.widget.Button
import android.widget.Toast
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import com.example.jogodavelha.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var binding:ActivityMainBinding

    //Vetor bidimensional que representará o tabuleiro de jogo
    val tabuleiro = arrayOf(
        arrayOf("A", "B", "C"),
        arrayOf("D", "E", "F"),
        arrayOf("G", "H", "I")
    )

    //Qual o Jogador está jogando
    var jogadorAtual = "Arrascaeta"

    override fun onCreate(savedInstanceState: Bundle?) {
        binding = ActivityMainBinding.inflate(layoutInflater)
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(binding.root)
    }

    //Função associada com todos os botões @param view é o botão clicado
    fun buttonClick(view: View) {
        //O botão clicado é associado com uma constante
        val buttonSelecionado = view as Button
        //O texto do botão recebe o jogador atual
        buttonSelecionado.text = jogadorAtual

        //De acordo com o botão clicado, a posição da matriz receberá o Jogador
        when(buttonSelecionado.id){
            binding.button1.id -> tabuleiro[0][0] = jogadorAtual
            binding.button2.id -> tabuleiro[0][1] = jogadorAtual
            binding.button3.id -> tabuleiro[0][2] = jogadorAtual
            binding.button4.id -> tabuleiro[1][0] = jogadorAtual
            binding.button5.id -> tabuleiro[1][1] = jogadorAtual
            binding.button6.id -> tabuleiro[1][2] = jogadorAtual
            binding.button7.id -> tabuleiro[2][0] = jogadorAtual
            binding.button8.id -> tabuleiro[2][1] = jogadorAtual
            binding.button9.id -> tabuleiro[2][2] = jogadorAtual
        }
        //Recebe o jogador vencedor através da função verificaTabuleiro. @param tabuleito
        var vencedor = verificaVencedor(tabuleiro)

        if(!vencedor.isNullOrBlank()) {
            Toast.makeText(this, "Vencedor: " + vencedor, Toast.LENGTH_LONG).show()
            val intent = Intent(this, MainActivity::class.java)
            startActivity(intent)
            finish()
        }

        if (jogadorAtual == "Arrascaeta") {
            buttonSelecionado.setBackgroundResource(R.drawable.arrascaeta)
            jogadorAtual = "Pedro"
        } else {
            buttonSelecionado.setBackgroundResource(R.drawable.pedroguilherme)
            jogadorAtual = "Arrascaeta"
        }
        buttonSelecionado.isEnabled=false
    }

    fun verificaVencedor(tabuleiro: Array<Array<String>>): String? {

        // Verifica linhas e colunas
        for (i in 0 until 3) {
            //Verifica se há três itens iguais na linha
            if (tabuleiro[i][0] == tabuleiro[i][1] && tabuleiro[i][1] == tabuleiro[i][2]) {
                return tabuleiro[i][0]
            }
            //Verifica se há três itens iguais na coluna
            if (tabuleiro[0][i] == tabuleiro[1][i] && tabuleiro[1][i] == tabuleiro[2][i]) {
                return tabuleiro[0][i]
            }
        }
        // Verifica diagonais
        if (tabuleiro[0][0] == tabuleiro[1][1] && tabuleiro[1][1] == tabuleiro[2][2]) {
            return tabuleiro[0][0]
        }
        if (tabuleiro[0][2] == tabuleiro[1][1] && tabuleiro[1][1] == tabuleiro[2][0]) {
            return tabuleiro[0][2]
        }
        //Verifica a quantidade de jogadores 
        var empate = 0
        for (linha in tabuleiro) {
            for (valor in linha) {
                if(valor.equals("X")||valor.equals("O")){
                    empate++
                }
            }
        }
        //Se existem 9 jogadas e não há três letras iguais, houve um empate
        if(empate == 9){
            return "Empate"
        }
        // Nenhum vencedor
        return null
    }
}
