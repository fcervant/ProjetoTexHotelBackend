<template>
  <div class="main">
    <div class="modalServicos2">
      <ModalServicos2
        :msg="msgModalServicos2"
        :idReservas="this.item.idReservas"
        :arrayServicosBD="arrayServicosBD"
        :botaoModalServicos="botaoModalServicos"
        :servicosSelection="servicosSelection"
      />
    </div>
    <div class="modalResumo2">
      <ModalResumo2
        :msg="msgModalResumo"
        :idReservas="idReservasModalResumo"
        :itemReservas="itemReservaModalResumo"
        :arrayServicosBD="arrayServicosBD"
      />
    </div>
    <div class="modalUsuarios">
      <ModalUsuarios :msg="msgModalUsuarios" :items="listClientes" />
    </div>

    <div class="sec">
      <div class="text-center">
        <h2>CADASTRO DE RESERVAS</h2>
        <p>*Campos de preenchimento obrigatório.</p>
      </div>

      <div class="shadow bg-light rounded-3 p-4 my-5 ">
        <p
          class="msgAlerta"
          id="msgAlerta"
          v-bind:style="{ color: msgAlertaColor }"
        >
          {{ msgAlerta }}
        </p>
        <hr />
        <form class="formReservas" action="" @submit.prevent>
          <div class="container">
            <div class="row my-2">
              <div class="col col-5 w-25">
                <label for="dataReserva" class="d-block fw-bold"
                  >*Data da reserva</label
                >
                <input
                  type="date"
                  id="dataReserva"
                  v-model="dataReserva"
                  placeholder="Data da reserva"
                  class="form-control w-75 ms-1"
                  :disabled="camposAtivos"
                />
              </div>
              <div class="col col-5 w-25">
                <label for="dataEntradaReserva" class="d-block fw-bold"
                  >*Data de Checkin</label
                >
                <input
                  type="date"
                  id="descricao"
                  v-model="dataEntradaReserva"
                  placeholder="Data Checkin Reserva"
                  class="form-control w-75 ms-1"
                  :disabled="camposAtivos"
                />
              </div>
              <div class="col col-5 w-25">
                <label for="dataSaidaReserva" class="d-block fw-bold"
                  >*Data de Checkout</label
                >
                <input
                  type="date"
                  id="label"
                  v-model="dataSaidaReserva"
                  placeholder="Data Checkout Reserva"
                  class="form-control w-75 ms-1"
                  :disabled="camposAtivos"
                />
              </div>
              <div class="col col-5 w-25">
                <label for="qtdHospedesReserva" class="d-block fw-bold"
                  >*Qtd Hospedes</label
                >
                <input
                  type="number"
                  id="qtdHospedesReserva"
                  v-model.number="qtdHospedesReserva"
                  placeholder="Qtd Hospedes"
                  class="form-control w-75 ms-1"
                  min="1"
                  :disabled="camposAtivos"
                />
              </div>

              <div class="col col-3">
                <label
                  for="statusReserva"
                  class="mt-3 d-block fw-bold"
                  v-if="itemArrayEdit"
                  >*Status Reserva</label
                >
                <!-- <input
                  type="text"
                  id="statusReserva"
                  v-model="statusReserva"
                  placeholder="Status da Reserva"
                  class="form-control"
                  v-if="itemArrayEdit"
                  :disabled="camposAtivos"
                /> -->

                <select
                  id="statusReserva"
                  v-model="statusReserva"
                  placeholder="Status da Reserva"
                  class="form-control ms-1 w-75"
                  v-if="itemArrayEdit"
                  :disabled="camposAtivos"
                >
                  <option value="Registrada">Registrada</option>
                  <option value="Confirmada">Confirmada</option>
                  <option value="Encerrada">Encerrada</option>
                  <option value="Cancelada">Cancelada</option>
                  <option value="Excluída">Excluída</option>
                </select>
              </div>
              <div class="mt-3 col col-4">
                <label
                  for="dataCancelamento"
                  class="d-block fw-bold"
                  v-if="itemArrayEdit"
                  ><small>Data cancelamento</small></label
                >
                <input
                  type="date"
                  id="dataCancelamento"
                  v-model="dataCancelamento"
                  placeholder="Data cancelamento reserva"
                  class="form-control w-50"
                  v-if="itemArrayEdit"
                  :disabled="true"
                />
              </div>

              <div class="mt-3 col col-5">
                <label
                  for="motivoCancelamento"
                  class="d-block fw-bold"
                  v-if="itemArrayEdit"
                  ><small>Motivo cancelamento</small></label
                >
                <input
                  type="text"
                  id="motivoCancelamento"
                  v-model="motivoCancelamento"
                  placeholder="Motivo cancelamento"
                  class="form-control me-auto w-75"
                  v-if="itemArrayEdit"
                  :disabled="camposAtivos"
                />
              </div>
            </div>

            <div class="container">
              <div class="row my-2 mt-3">
                <div class="col col-5 me-5">
                  <!-- <label for="idUsuario" class="d-block fw-bold" @click="handleClick('usuarios')">ID Usuario - clique para consulta</label> -->
                  <span><b>ID Usuario - clique para consulta </b></span
                  ><img
                    src="../../assets/search-persons.png"
                    alt="Pesquisa clientes"
                    style="width: 25px; height: 30px"
                    @click="handleClick('usuarios')"
                  />
                  <i class="fa fa-search" aria-hidden="true"></i>
                  <input
                    type="number"
                    id="idUsuario"
                    v-model.number="idUsuario"
                    placeholder="id do Usuario"
                    class="form-control"
                    @change="changeUserId"
                    :disabled="campoAtivoIdUsuario"
                    style="display: inline-block"
                  />
                  <input
                    type="text"
                    id="nomeUsuario"
                    v-model="nomeUsuario"
                    placeholder="Nome Usuario"
                    class="form-control"
                    :disabled="true"
                  />

                  <label for="idAcomodacao" class="d-block fw-bold"
                    >ID Acomodacao</label
                  >
                  <input
                    type="number"
                    id="idAcomodacao"
                    v-model.number="idAcomodacao"
                    placeholder="id da Acomodacao"
                    class="form-control"
                    @change="changeAcomodacaoId"
                    :disabled="camposAtivos"
                  />
                  <input
                    type="text"
                    id="acomodacaoTipo"
                    v-model="acomodacaoTipo"
                    placeholder="Tipo Acomodação"
                    class="form-control"
                    :disabled="true"
                  />
                  <input
                    type="text"
                    id="acomodacaoVlrDiaria"
                    v-model="acomodacaoVlrDiaria"
                    placeholder="Vlr Diaria Acomodação"
                    class="form-control"
                    :disabled="true"
                  />
                </div>
                <div class="col col-5 mx-auto">
                  <label for="qtDiarias" class="d-block fw-bold"
                    >Qtdade Diarias</label
                  >
                  <input
                    type="text"
                    id="qtDiarias"
                    v-model="qtDiarias"
                    placeholder="Qtdade Diarias"
                    class="form-control"
                    :disabled="true"
                  />
                  <label for="valorReserva" class="d-block fw-bold"
                    >Valor Total Diarias</label
                  >
                  <input
                    type="number"
                    id="valorReserva"
                    v-model.number="valorReserva"
                    placeholder="Valor das Diarias"
                    class="form-control"
                    :disabled="true"
                  />
                  <label for="valorTotalServicos" class="d-block fw-bold"
                    >Valor Total Serviços</label
                  >
                  <input
                    type="number"
                    id="valorTotalServicos"
                    v-model.number="valorTotalServicos"
                    placeholder="Valor dos Serviços"
                    class="form-control"
                    :disabled="true"
                  />
                  <label for="valorTotalDesconto" class="d-block fw-bold"
                    >Valor Total Reserva</label
                  >
                  <input
                    type="number"
                    id="valorTotalDesconto"
                    v-model.number="valorTotalDesconto"
                    placeholder="Valor Total Reserva"
                    class="form-control"
                    :disabled="true"
                  />
                  <label for="cupom" class="d-block fw-bold"
                    >Cupom Desconto</label
                  >
                  <input
                    type="text"
                    id="cupom"
                    v-model="cupom"
                    placeholder="Cupom desconto"
                    class="form-control"
                    :disabled="true"
                  />
                </div>
                <div class="col col-5 me-5"></div>
              </div>
            </div>
          </div>

          <div class="btn-group" role="group">
            <button
              v-if="showSalvarButton"
              @click="handleClick('salvar')"
              class="button mt-2"
              :disabled="camposAtivos"
            >
              Cadastrar
            </button>
            <button
              v-if="showGerarCupomButton"
              @click="handleClick('gerarCupom')"
              class="button mt-2"
              :disabled="camposAtivos"
            >
              Gerar Cupom
            </button>
            <button
              v-if="showCancelarReservaButton"
              @click="handleClick('cancelarReserva')"
              id="btnCancelarReserva"
              class="button mt-2"
              type="button"
            >
              Cancelar Reserva
            </button>
            <button
              v-if="showExcluirButton"
              @click="handleClick('excluir')"
              id="btnExcluir"
              class="button mt-2"
              type="button"
              :disabled="camposAtivos"
            >
              Excluir Reserva
            </button>
            <!-- <button
              v-if="showModalServicos"
              @click="handleClick('usuarios')"
              id="btnUsuarios"
              class="button mt-2"
              type="button"
            >
              Usuários
            </button> -->
            <button
              v-if="showModalServicos"
              @click="handleClick('servicos')"
              id="btnServicos"
              class="button mt-2"
              type="button"
            >
              Serviços
            </button>
            <button
              v-if="showModalResumo"
              @click="handleClick('resumo')"
              id="btnCancelar"
              class="button mt-2"
              type="button"
            >
              Resumo
            </button>
            <button
              v-if="showCancelarButton"
              @click="handleClick('cancelar')"
              id="btnCancelar"
              class="button mt-2"
              type="button"
            >
              Sair
            </button>
          </div>
        </form>
      </div>
      <div class="listReservas table-responsive table-responsive-sm">
        <table class="table table-striped">
          <thead>
            <th scope="col">ID</th>
            <th scope="col">Id User</th>
            <th scope="col">Dt Reserva</th>
            <th scope="col">CheckIn</th>
            <th scope="col">CheckOut</th>
            <th scope="col">Qt Hospedes</th>
            <th scope="col">Vlr Total</th>
            <th scope="col">Status</th>
          </thead>
          <tbody>
            <tr scope="row" v-for="item in items" :key="item.idReservas">
              <td>{{ item.idReservas }}</td>
              <td>{{ item.usuario_idUsuario }}</td>
              <td>{{ item.dataReserva }}</td>
              <td>{{ item.dataEntradaReserva }}</td>
              <td>{{ item.dataSaidaReserva }}</td>
              <td>{{ item.qtdHospedesReserva }}</td>
              <td>{{ item.valorTotalDesconto }}</td>
              <td>{{ item.statusReserva }}</td>
              <div class="handleItem w-30 border px-3">
                <button
                  @click="handleItem('editar', item.idReservas)"
                  id="btnEditar"
                  class="button me-3"
                  type="button"
                  title="Edita a reserva"
                >
                  Editar
                </button>
                <button
                  @click="handleItem('excluir', item.idReservas)"
                  id="btnEditar"
                  class="button"
                  type="button"
                  title="Exclui a reserva"
                >
                  Excluir
                </button>
              </div>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
// a ordem dos imports faz diferença
// artigo https://stackoverflow.com/questions/27064176/typeerror-modal-is-not-a-function-with-bootstrap-modal

// jquery primeiro, pois modal do bootstrap depende disso...
var jQuery = require("jquery");
window.jQuery = jQuery;
window.$ = jQuery;

// bootstrap
//import "bootstrap/dist/css/bootstrap.css";
//import "bootstrap/dist/js/bootstrap.js";
//const bootstrap = require("bootstrap");

import axios from "axios";
import { Reservas } from "@/../adm/src/types/reservas/Reservas.js";
import ModalServicos2 from "@/../adm/src/components/reserva/ModalServicos2";
import ModalResumo2 from "@/../adm/src/components/reserva/ModalResumo2";
import ModalUsuarios from "@/../adm/src/components/reserva/ModalUsuarios";
import * as mainFunc from "@/../adm/src/types/reservas/MainFunctions.js";

// testar currency via import VueCurrencyInput from 'vue-currency-input';
// site https://vue-currency-input-v1.netlify.app/guide/#installation
// import { currencyFormat } from "@/../src/components/reserva/FormReserva.vue";
// import ModalResumo from "./ModalResumo";

import { mapState } from "vuex";

export default {
  name: "ReservasView.view",
  components: {
    // components....
    ModalServicos2,
    ModalResumo2,
    ModalUsuarios,
  },
  data() {
    // data
    return {
      message: "TabTest",
      tab: null,
      // msg1: "msg1",
      // msg2: "msg2",
      inputValue: "Teste",
      msgAlerta: "Mensagens do sistema",
      msgAlertaColor: "green", //  cor inicial da mensagem - sera trocar por red em caso de erros...
      flagSalvarOk: true, // flag para controle de inclusão e alteração registros
      idReservas: "",
      msgModalServicos2: "",
      arrayServicosReserva: [],
      arrayServicosBD: [],
      msgModalResumo: "",
      msgModalUsuarios: "",
      idReservasModalResumo: "",
      itemReservaModalResumo: [],
      dataReserva: new Date().toISOString().substring(0, 10),
      dataEntradaReserva: new Date().toISOString().substring(0, 10),
      dataSaidaReserva: new Date().toISOString().substring(0, 10),
      valorReserva: "",
      qtdHospedesReserva: 1,
      idUsuario: "",
      nomeUsuario: "",
      idAcomodacao: "",
      acomodacaoTipo: "",
      acomodacaoVlrDiaria: 0,
      acomodacaoQtMaxPessoas: 1,
      qtDiarias: 0,
      statusReserva: "Registrada",
      dataCancelamento: new Date().toISOString().substring(0, 10),
      motivoCancelamento: "",
      cupom: "",
      taxaDescontoCupom: 10,
      valorTotalDesconto: 0,
      valorTotalServicos: 0,
      item: [],
      items: [],
      itemServico: [],
      idServico: "",
      nomeServico: "",
      itemUsuario: [],
      itemAcomodacao: [],
      itemArrayReservas: 0,
      itemArrayEdit: false,
      camposAtivos: false,
      botaoModalServicos: false,
      campoAtivoIdUsuario: false,
      showSalvarButton: true,
      showCancelarReservaButton: false,
      showExcluirButton: false,
      showCancelarButton: false,
      showModalServicos: false,
      showModalResumo: false,
      showGerarCupomButton: false,
      servicosSelection: false,
      listClientes: "",
    };
  },

  created() {
    //
    this.getReservas();
    console.log("Created:", this.itens);
  },

  beforeMount() {
    this.checkLogin();
    console.log(this.checkLogin());
  },
  setup() {
    // setup...
  },

  methods: {
    checkLogin() {
      if (localStorage.getItem("loginStatus")) {
        if (localStorage.getItem("loginStatus") == "admin") return true;
        else if (localStorage.getItem("loginStatus") == "cliente")
          this.$router.push("/");
        return true;
      } else {
        this.$router.push("/");
        return false;
      }
    },

    changeUserId() {
      // mudou o id do usuario, dispara api para checar validade...
      this.validaUsuariosById(this.idUsuario);
    },

    changeAcomodacaoId() {
      // mudou o id do usuario, dispara api para checar validade...
      this.validaAcomodacaoById(this.idAcomodacao);

      let idOK = true;
      console.log(
        "Acomodacao",
        this.acomodacaoTipo,
        this.acomodacaoVlrDiaria,
        this.acomodacaoQtMaxPessoas
      );
      return idOK;
    },

    // verificar usuario pelo id
    async validaUsuariosById(idUsuario) {
      const token = sessionStorage.getItem('token');
      try {
        const response = await axios.get(
          `http://localhost:5000/usuario/${idUsuario}`,{
          headers: {
          'Authorization': `Bearer ${token}`
        }});
        this.itemUsuario = response.data;
        if (idUsuario === response.data.idUsuario) {
          this.nomeUsuario = response.data.nomeUsuario;
        } else {
          this.nomeUsuario = "Usuario inválido - Verificar!!";
        }
        //console.log("getServicosById", this.itemServico);
        return response;
      } catch (err) {
        console.log(err);
      }
    },

    // carrega lista de usuarios clientes
    async getUsersCliente() {
      const token = sessionStorage.getItem('token');
      try {
        const response = await axios.get(
          "http://localhost:5000/usuario/cliente",{
          headers: {
          'Authorization': `Bearer ${token}`
        }});
        console.log("getUsersCliente", response);
        this.listClientes = response.data;
        return response;
      } catch (err) {
        console.log(err);
      }
    },

    // verificar acomodação  pelo id - ok!
    async validaAcomodacaoById(idAcomodacao) {
      const token = sessionStorage.getItem('token');
      try {
        const response = await axios.get(
          `http://localhost:5000/acomodacao/${idAcomodacao}`,{
          headers: {
          'Authorization': `Bearer ${token}`
        }});
        this.itemAcomodacao = response.data;
        if (idAcomodacao === response.data.idAcomodacao) {
          this.acomodacaoTipo = response.data.tipoAcomodacao;
          this.acomodacaoVlrDiaria = response.data.valorAcomodacao;
          this.acomodacaoQtMaxPessoas = response.data.qtMaxPessoas;
        } else {
          this.acomodacaoTipo = "Acomodação inválida - Verificar!!";
          this.acomodacaoVlrDiaria = 0;
          this.acomodacaoQtMaxPessoas = 1;
        }
        return response;
      } catch (err) {
        console.log(err);
      }
    },

    // Lista todas as reservas - ok!
    async getReservas() {
      console.log("ReservasView...getReservas()");
      const token = sessionStorage.getItem('token');
      try {
        const response = await axios.get("http://localhost:5000/reserva" ,{
          headers:{
            'Authorization': `Bearer ${token}`
          }});
        this.items = response.data;
        // console.log("getReservas - atualizando itens da lista", this.items);
        return response.data;
      } catch (err) {
        console.log(err);
      }
    },

    // localiza servico pelo id
    async getReservasById(idReservas) {
      console.log("idReservas = ", idReservas);
      const token = sessionStorage.getItem('token');
      try {
        const response = await axios.get(
          `http://localhost:5000/reserva/${idReservas}`,{
          headers:{
            'Authorization': `Bearer ${token}`
          }});
        this.item = response.data;
        this.idReservas = this.item.idReservas;
        this.dataReserva = new Date(this.item.dataReserva)
          .toISOString()
          .substring(0, 10);
        this.dataEntradaReserva = new Date(this.item.dataEntradaReserva)
          .toISOString()
          .substring(0, 10);
        this.dataSaidaReserva = new Date(this.item.dataSaidaReserva)
          .toISOString()
          .substring(0, 10);
        this.qtdHospedesReserva = this.item.qtdHospedesReserva;
        this.valorReserva = this.item.valorReserva;
        this.valorTotalServicos = this.item.valorTotalServicos;
        this.valorTotalDesconto = this.item.valorTotalDesconto;
        this.qtDiarias = this.item.qtDiarias;
        this.cupom = this.item.cupom;
        this.taxaDescontoCupom = this.item.taxaDescontoCupom;
        this.idUsuario = this.item.usuario_idUsuario;
        this.nomeUsuario = this.item.nomeUsuario;
        this.idAcomodacao = this.item.acomodacoes_idAcomodacao;
        this.acomodacaoTipo = this.item.nomeAcomodacao;
        this.acomodacaoVlrDiaria = this.item.valorAcomodacao;
        this.acomodacaoQtMaxPessoas = this.item.valorAcomodacao;
        this.statusReserva = this.item.statusReserva;
        this.dataCancelamento = new Date(this.item.dataCancelamento)
          .toISOString()
          .substring(0, 10);
        this.motivoCancelamento = this.item.motivoCancelamento;
        if (
          this.statusReserva === "Cancelada" ||
          this.statusReserva === "Encerrada" ||
          this.statusReserva === "Excluída"
        ) {
          this.camposAtivos = true;
          this.botaoModalServicos = true;

          // desativa seleção de serviços na modal de serviços...
          this.servicosSelection = true;
          alert(
            "O status desta reserva não permite mais edição ou exclusão! Para reativar a reserva entre em contato com o gerente do Hotel"
          );
        } else {
          this.camposAtivos = false;
          this.botaoModalServicos = false;
          this.servicosSelection = false;
        }
        console.log(
          "this.acomodacaoTipo",
          this.statusReserva,
          this.acomodacaoTipo,
          this.acomodacaoVlrDiaria
        );
        return response.data;
      } catch (err) {
        console.log(err);
      }
    },

    handleClick(action) {
      if (action == "salvar") {
        console.log("Etapa 1 - click Cadastrar");
        // this.msg1 = "Cliquei handleClick salvar...";
        // this.msg2 = `Status itemArrayEdit=${this.itemArrayEdit}`;

        // executa validação dos campos...
        let validadorDados, dadosOk, msgRetorno;
        validadorDados = checkInfo(
          this.idUsuario,
          this.idAcomodacao,
          this.dataReserva,
          this.dataEntradaReserva,
          this.dataSaidaReserva,
          this.qtdHospedesReserva,
          this.acomodacaoTipo,
          this.acomodacaoQtMaxPessoas
        );
        dadosOk = validadorDados[0];
        msgRetorno = validadorDados[1];

        // exibe mensagem de erro se necessário, aborta persistência...
        if (dadosOk == false) {
          this.msgAlerta = `${msgRetorno}`;
          this.msgAlertaColor = "red";
          return false;
        }

        if (this.itemArrayEdit == true) {
          this.arrayServicos = this.arrayServicosBD;
          console.log("Etapa 3.1 - this.arrayServicos de arrayServicosBD",this.arrayServicos);
        } else {
          this.arrayServicos = this.Servicos2.data;
          console.log("Etapa 3.1 - this.arrayServicos de Servicos2.data",this.arrayServicos);
        }

        console.log("Etapa 3.2 - arrayServicos",this.arrayServicos);
        let reserva = new Reservas();

        reserva.salvar(
          this.idReservas,
          this.dataReserva,
          this.dataEntradaReserva,
          this.dataSaidaReserva,
          this.valorReserva,
          this.qtdHospedesReserva,
          this.idUsuario,
          this.idAcomodacao,
          this.acomodacaoTipo,
          this.acomodacaoVlrDiaria,
          this.qtDiarias,
          this.statusReserva,
          this.dataCancelamento,
          this.motivoCancelamento,
          this.cupom,
          this.taxaDescontoCupom,
          this.valorTotalDesconto,
          this.valorTotalServicos,
          this.acomodacaoQtMaxPessoas,
          this.itemArrayReservas,
          this.itemArrayEdit,
          // this.arrayServicosBD, // checar e excluir
          this.arrayServicos
        );

        // recarrega lista de serviços
        this.getReservas();
        // força atualização pela segunda vez, no caso de edição não está atualizando com um unica chamada, checar...
        this.getReservas();
        console.log("Etapa 4 - limpar a localStorage");
        localStorage.removeItem("servicosEscolhidos");
      }

      if (action == "excluir") {
        // chama rotina de exclusão, desabilita o botão e limpa os campos na rotina que já está abaixo...
        // this.msg1 = "Cliquei handleClick excluir...";
        // this.msg2 = `this.idReservas=${this.idReservas}`;
        let conf = confirm(
          "Confirma exclusão da reserva? Esta operação não poderá ser desfeita!"
        );
        if (conf) {
          let reserva = new Reservas();
          reserva.excluir(this.idReservas);
          // recarrega lista de serviços
          this.getReservas();
        }
      }

      if (action == "servicos") {
        this.arrayServicosBD = this.ServicosReserva;
        window.$("#modalServicos2").modal("show");
        this.msgModalServicos2 =
          "Baixa utilizaçao serviço 5G - ofereça desconto na reserva";
        this.$store.dispatch("ServicosReserva/getData", {
          idReserva: `${this.idReservas}`,
        });
        this.campoAtivoIdUsuario = true;
        return true;
      }

      if (action == "resumo") {
        this.$store.dispatch("ServicosReserva/getData", {
          idReserva: `${this.idReservas}`,
        });
        this.arrayServicosBD = this.ServicosReserva;
        this.idReservasModalResumo = this.idReservas;
        this.itemReservaModalResumo = this.item;
        console.log("Resumo", this.itemReservaModalResumo);
        this.msgModalResumo = "Sem mensagens no momento.";
        // this.$store.dispatch("ServicosReserva/getData", { idReserva: `${this.idReservas}` });
        window.$("#modalResumo2").modal("show");
        this.campoAtivoIdUsuario = true;
        return true;
      }

      if (action == "usuarios") {
        if (this.campoAtivoIdUsuario !== true) {
          this.getUsersCliente();
          window.$("#modalUsuarios").modal("show");
          this.campoAtivoIdUsuario = false;
        }
        return true;
      }

      if (action == "gerarCupom") {
        if (this.cupom === "" || this.cupom == "Sem desconto") {
          let conf = confirm("Confirma geração de cupom?");
          console.log("Conf...:", conf);
          if (conf === true) {
            this.cupom = mainFunc.geraCupomDesconto();
            this.taxaDescontoCupom = 10;
            console.log("Vou gerar cupom", this.cupom);
            let conf = confirm(
              `Cupom gerado e autorizado - ${this.cupom} - deseja aplicar o mesmo na reserva?\nApós o desconto alterações nos dados da reserva requerem atenção!`
            );
            if (conf === true) {
              // força calculo antes de salvar...
              this.arrayServicosBD = this.ServicosReserva;
              // this.calculaReserva();
              this.handleClick("salvar");
            } else {
              this.cupom = "";
            }
          }
        } else {
          alert("Um cupom de desconto já foi aplicado nesta reserva!");
          this.cupom === ""
            ? (this.showGerarCupomButton = true)
            : (this.showGerarCupomButton = false);
        }
        return true;
      }

      if (action == "cancelar") {
        //this.msg1 = "Cliquei handleClick sair tela edição";
        console.log("Cliquei handleClick sair tela edição...", action);
        console.log("Testando mainFunc...:", mainFunc.currencyFormat("2000"));
        // atualiza campos do formulário / tela
        // this.getReservasById(this.idReservas);
        // this.itemArrayEdit = false;
        //this.showSalvarButton = true;
        //this.showCancelarButton = false;
        //this.showCancelarReservaButton = false;
        // this.showExcluirButton = false;
        //this.showModalServicos = false;
        //this.showModalResumo = false;
        this.campoAtivoIdUsuario = false;
        this.camposAtivos = false;
        // return true;
      }

      // após inclusão, limpa campos do form...
      this.msgAlerta = "Mensagens do sistema";
      this.msgAlertaColor = "green";
      this.dataReserva = new Date().toISOString().substring(0, 10);
      this.dataEntradaReserva = new Date().toISOString().substring(0, 10);
      this.dataSaidaReserva = new Date().toISOString().substring(0, 10);
      this.valorReserva = "";
      this.qtdHospedesReserva = "";
      this.idUsuario = "";
      this.nomeUsuario = "";
      this.idAcomodacao = "";
      this.acomodacaoTipo = "";
      this.acomodacaoVlrDiaria = 0;
      this.qtDiarias = 0;
      this.statusReserva = "Registrada"; // status inicial da Reserva
      this.dataCancelamento = new Date().toISOString().substring(0, 10);
      (this.motivoCancelamento = ""),
        (this.cupom = ""),
        (this.taxaDescontoCupom = 0),
        (this.valorTotalDesconto = 0),
        (this.valorTotalServicos = 0),
        // retorna status dos botões...
        (this.showSalvarButton = true);
      this.showCancelarButton = false;
      this.showCancelarReservaButton = false;
      this.showExcluirButton = false;
      this.showModalServicos = false;
      this.showModalResumo = false;
      this.showGerarCupomButton = false;
      this.itemArrayEdit = false;
      this.campoAtivoIdUsuario = false;
      this.camposAtivos = false;

      this.getReservas();
      return true;
    },

    handleItem(action, idReservas) {
      console.log("Entrei no handleItem");
      if (action == "editar") {
        this.msg1 = "Cliquei HandleItem edit";
        console.log("Entrei no handleItem...", action);
        this.campoAtivoIdUsuario = true;
        this.getReservasById(idReservas);
        this.msgAlerta = `Reserva: ${idReservas}`;
        this.itemArrayEdit = true;
        // contorle de edição do user
        this.showSalvarButton = true;
        this.showCancelarButton = true;
        this.showCancelarReservaButton = false;
        this.showExcluirButton = false;
        this.showModalServicos = true;
        this.showModalResumo = true;
        this.msg2 = `Status itemArrayEdit=${this.itemArrayEdit}`;
        this.arrayServicosBD = this.ServicosReserva;
        // checar controle  aqui
        this.showGerarCupomButton = true;
        // checar controle  aqui
      }
      if (action == "excluir") {
        // this.msg1 = "Cliquei HandleItem excluir";
        console.log("Entrei no handleItem...", action);
        // atualiza campos do formulário
        this.getReservasById(idReservas);
        this.itemArrayEdit = false;
        this.campoAtivoIdUsuario = true;
        this.showSalvarButton = false;
        this.showCancelarButton = true;
        this.showCancelarReservaButton = false;
        this.showExcluirButton = true;
        this.showModalServicos = true;
        this.showModalResumo = true;
      }
      if (action == "cancelar") {
        this.msg1 = "Cliquei HandleItem cancelar reserva";
        console.log("Entrei no handleItem...", action);
        // atualiza campos do formulário
        this.getReservasById(idReservas);
        this.itemArrayEdit = false;
        this.campoAtivoIdUsuario = false;
        this.showSalvarButton = false;
        this.showCancelarButton = true;
        this.showCancelarReservaButton = true;
        this.showExcluirButton = false;
        this.showModalServicos = false;
        this.showModalResumo = false;
        this.showModalServicos = false;
        this.campoAtivoIdUsuario = false;
        return true;
      }

      this.msg2 = `idReservas ${idReservas}`;
      //this.campoAtivoIdUsuario = true;
    },
  },
  watch: {
    // isso funciona, mas executa a cada novo caracter, e não somente depois de um tab ou enter...
    // idUsuario: function (val) {
    //   console.log("Mudou valor id usuario. ID: ", val);
    // },
  },
  computed: {
    // incluir funções...
    ...mapState(["Servicos2"]),
    ...mapState(["ServicosReserva"]),

    changeBackground(color) {
      document.body.style.background = color;
      return true;
    },
  },
  mounted() {
    // funções mounted...
    console.log("Passando pelo mounted...");
    this.$store.dispatch("Servicos2/getData");
  },
};

export function checkInfo(
  idUsuario,
  idAcomodacao,
  dataReserva,
  dtEntrada,
  dtSaida,
  qtdHospedesReserva,
  acomodacaoTipo,
  acomodacaoQtMaxPessoas
) {
  let msgReturn;
  msgReturn = [true, "Dados OK!"];
  console.log(
    idUsuario,
    idAcomodacao,
    dataReserva,
    dtEntrada,
    dtSaida,
    qtdHospedesReserva,
    acomodacaoTipo,
    acomodacaoQtMaxPessoas
  );

  if (idUsuario === "") {
    msgReturn = [false, "Usuario inválido"];
    return msgReturn;
  }

  if (idAcomodacao === "") {
    msgReturn = [false, "Acomodação inválida"];
    return msgReturn;
  }

  if (
    dtEntrada < dataReserva ||
    dtEntrada == "" ||
    dtSaida == "" ||
    dtSaida <= dtEntrada
  ) {
    msgReturn = [false, "Datas de entrada e/ou saída inválidas"];
    return msgReturn;
  }

  if (qtdHospedesReserva == 0 || qtdHospedesReserva > acomodacaoQtMaxPessoas) {
    msgReturn = [
      false,
      `Quantidade hóspedes inválida - suite ${acomodacaoTipo} suporta no máximo ${acomodacaoQtMaxPessoas} pessoas.`,
    ];
    return msgReturn;
  }

  // console.log("msgReturn...",msgReturn)
  console.log("Etapa 2 - checar dados da reserva");
  return msgReturn;
}
</script>

<style scoped>
@charset "UTF-8";
@import url("https://fonts.googleapis.com/css?family=Poppins:300,400,500,600,700,800,900&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

.column {
  float: left;
  width: 50%;
}

.inline-link {
  display: inline-block;
}

/* Clear floats after the columns */
.row:after {
  content: "";
  display: table;
  clear: both;
}

.sec {
  position: relative;
  padding: 2vw;
  transition: all 0.3s ease;
  color: black;
}

.sec > div {
  max-width: 90%;
  margin: 2% 5%;
}

.flex {
  display: flex;
  flex-wrap: wrap;
  max-width: 90%;
  margin: 0 5%;
}

.flex > div {
  flex: 1 1 420px;
  margin: 10px;
}
.button {
  background: transparent;
  color: black;
  padding: 5px;
  border-radius: 50px;
  cursor: pointer;
  overflow: hidden;
  margin: 5px;
}

.button:hover {
  background: black;
  color: #fff;
  border-radius: 50px;
  padding: 5px;
}
</style>
