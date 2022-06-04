<script setup>
import DataTable from './components/DataTable.vue'
import { onMounted, reactive, ref } from 'vue'

import Papa from 'papaparse'

const uploadCsv = ref(null)
let row = reactive({})

// const buttonUpload = document.getElementById('upload-csv')
onMounted(() => {
  // var buttonUpload = document.getElementById('upload-csv')
  console.log('created')
  console.log(uploadCsv)
})

function uploadCsvFile() {
  Papa.parse(uploadCsv.value.files[0], {
    download: true,
    header: true,
    complete: function (results) {
      row = results
      console.log(row.data)
    }
  })
}

function downloadCsvFile() {
  console.log(row.data)
}

const searchQuery = ref('')
const gridColumns = [
  'Nome_Fantasia',
  'Registro_ANS',
  'Data_Registro_ANS',
  'CNPJ',
  'Razao_Social',
  'Modalidade',
  'Logradouro',
  'Numero',
  'Complemento',
  'Bairro',
  'Cidade',
  'UF',
  'CEP',
  'DDD',
  'Telefone',
  'Fax',
  'Endereço_eletrônico',
  'Representante',
  'Cargo_Representante'
]
const gridData = [
  {
    Registro_ANS: '418374',
    CNPJ: '11828089000103',
    Razao_Social:
      ' CAIXA DE ASSIST�NCIA DO SETOR DE ENERGIA -EVIDA -ASSIST�NCIA � SA�DE',
    Nome_Fantasia: 'E-VIDA',
    Modalidade: 'Autogest�o',
    Logradouro: 'SETOR DE HABITA��ES COLETIVAS GEMINADAS ',
    Numero: 'QUADRA 704/705',
    Complemento: 'BLOCO C, LOJA 48',
    Bairro: 'ASA NORTE',
    Cidade: 'Bras�lia',
    UF: 'DF',
    CEP: '70730630',
    DDD: '61',
    Telefone: '39668300',
    Fax: '39668302',
    Endereço_eletrônico: 'governanca@evida.org.br',
    Representante: 'ELI PINTO DE MELO JUNIOR',
    Cargo_Representante: 'PRESIDENTE',
    Data_Registro_ANS: '12/01/2012'
  },
  {
    Registro_ANS: '314668',
    CNPJ: '17505793000101',
    Razao_Social:
      'ABERTTA SA�DE - ASSOCIA��O BENEFICENTE DOS EMPREGADOS DA ARCELORMITTAL NO BRASIL',
    Nome_Fantasia: 'ABERTTA SA�DE',
    Modalidade: 'Autogest�o',
    Logradouro: 'AV. BERNARDO MONTEIRO',
    Numero: '831',
    Complemento: 'Subsolo, 2� andar e 3� andar',
    Bairro: 'SANTA EFIG�NIA',
    Cidade: 'Belo Horizonte',
    UF: 'MG',
    CEP: '30150281',
    DDD: '31',
    Telefone: '32484300',
    Fax: '32484377',
    Endereço_eletrônico: 'abertta.ans@arcelormittal.com.br',
    Representante: 'JOS� ANT�NIO GIMENEZ',
    Cargo_Representante: 'PRESIDENTE',
    Data_Registro_ANS: '28/12/1998'
  }
]
</script>

<template>
  <div class="container">
    <div class="title">
      <h1>Intuitive Care</h1>
    </div>
    <div class="content">
      <div class="content-header">
        <div class="content-upload">
          <input
            ref="uploadCsv"
            class="upload-csv"
            type="file"
            id="upload-csv"
            accept=".csv"
          />
          <div class="buttons">
            <button @click="uploadCsvFile" id="btn-upload-csv">
              Carregar CSV
            </button>
            <button @click="downloadCsvFile" id="btn-download-csv">
              Baixar CSV
            </button>
          </div>
        </div>
        <form id="search">
          Buscar Cadastros
          <input name="query" v-model="searchQuery" />
        </form>
      </div>
      <DataTable
        class="table"
        :data="gridData"
        :columns="gridColumns"
        :filter-key="searchQuery"
      >
      </DataTable>
    </div>
  </div>
</template>

<style lang="scss">
* {
  margin: 0;
}
.container {
  background: whitesmoke;
  height: 100vh;
  display: grid;
  place-items: center;
  // grid-template-columns: 800px;
  grid-template-rows: max-content max-content;

  .title {
    width: 100%;
    display: grid;
    place-items: center;
    height: 70px;
    background: white;
    // text-align: center;
    // align-self: center;
    box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
  }

  .content {
    // width: 800px;
    // height: 500px;
    padding: 1rem;
    border-radius: 10px;
    margin: 1rem;
    background: white;

    .content-header {
      display: grid;
      grid-template-columns: 1fr 1fr;
      // justify-items: end;
      align-items: center;
    }

    .content-upload {
      display: grid;
    }

    #search {
      justify-self: end;
    }

    .table {
      display: grid;
      grid-template-columns: 800px;
      grid-template-rows: 500px;
      overflow-y: scroll;
    }
  }
}
</style>
