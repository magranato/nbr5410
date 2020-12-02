<template>
  <div id="app" class="w-full">
    <div
      class="block font-medium w-full p-2 mb-4 text-lg bg-blue-400 text-white text-center"
    >
      <div>NBR 5410</div>
      <div class="font-normal text-sm">Dimensionamento de Eletrodutos</div>
    </div>
    <div
      class="grid grid-rows-2 lg:grid-cols-2 divide-y lg:divide-y-0 divide-x-0 lg:divide-x divide-gray-200"
    >
      <div class="p-4">
        <div class="w-full text-center text-lg font-medium text-blue-400 mb-2">
          Lista de Cabos
        </div>
        <div class="overflow-hidden border border-gray-200 sm:rounded-lg mb-2">
          <table class="min-w-full divide-y divide-gray-200">
            <thead>
              <tr class="divide-x">
                <th
                  scope="col"
                  class="w-1/12 px-6 py-3 bg-gray-100 text-center text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  &#35;
                </th>
                <th
                  scope="col"
                  class="w-8/12 px-6 py-3 bg-gray-100 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Formação
                </th>
                <th
                  scope="col"
                  class="w-1/6 px-6 py-3 bg-gray-100 text-center text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  <span class="hidden lg:block">Quantidade</span>
                  <span class="block lg:hidden">Qtde.</span>
                </th>
                <th scope="col" class="w-1/12 px-6 py-3 bg-gray-100">
                  <span
                    v-if="!cabos.length"
                    class="hidden lg:block text-gray-300 text-center text-xs font-medium uppercase tracking-wider"
                    >Limpar</span
                  >
                  <button
                    v-else
                    @click="limpar"
                    class="hidden lg:block text-blue-400 cursor-pointer focus:outline-none text-center text-xs font-medium uppercase tracking-wider"
                  >
                    Limpar
                  </button>

                  <span
                    v-if="!cabos.length"
                    class="block lg:hidden text-gray-300 text-center text-xs font-medium uppercase tracking-wider"
                  >
                    <svg
                      class="h-6 w-6"
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke="currentColor"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
                      />
                    </svg>
                  </span>
                  <button
                    v-else
                    @click="limpar"
                    class="block lg:hidden text-blue-400 cursor-pointer focus:outline-none text-center text-xs font-medium uppercase tracking-wider"
                  >
                    <svg
                      class="h-6 w-6"
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke="currentColor"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
                      />
                    </svg>
                  </button>
                </th>
              </tr>
            </thead>
            <tbody class="bg-white divide-y divide-gray-200">
              <tr v-if="cabos.length == 0">
                <td
                  colspan="4"
                  class="text-gray-700 text-center py-2 text-sm lg:text-base"
                >
                  Nenhum cabo adicionado
                </td>
              </tr>
              <tr v-else v-for="(cabo, i) in cabos" :key="i" class="divide-x">
                <td
                  class="p-3 whitespace-nowrap text-sm lg:text-base text-gray-700 text-center"
                >
                  {{ i + 1 }}
                </td>
                <td
                  class="p-3 whitespace-nowrap text-sm lg:text-base text-gray-700"
                >
                  <select
                    v-model="cabo.index"
                    autocomplete="off"
                    class="w-full py-2 px-3 border border-gray-300 bg-white rounded-md focus:outline-none focus:ring-blue-400 focus:border-blue-400 sm:text-sm"
                  >
                    <option value="-1">Selecione um cabo</option>
                    <option
                      v-for="(item, j) in listaCabos"
                      :key="i + item.formacao"
                      :value="j"
                    >
                      {{ item.formacao }}
                    </option>
                  </select>
                </td>
                <td
                  class="p-3 whitespace-nowrap text-sm lg:text-base text-gray-700"
                >
                  <input
                    type="number"
                    min="0"
                    step="1"
                    v-model="cabo.qtde"
                    class="w-full py-2 px-3 border border-gray-300 bg-white rounded-md focus:outline-none focus:ring-blue-400 focus:border-blue-400 sm:text-sm"
                  />
                </td>
                <td class="p-3 whitespace-nowrap text-center font-medium">
                  <button
                    @click="excluir(i)"
                    class="flex w-full justify-center text-gray-500 focus:outline-none"
                  >
                    <svg
                      class="h-6 w-6"
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke="currentColor"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M6 18L18 6M6 6l12 12"
                      />
                    </svg>
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="w-full text-gray-700 text-base text-right mb-2">
          {{ nCabos + " cabo" + (nCabos > 1 ? "s" : "") }}
        </div>
        <button
          class="flex items-center justify-center focus:outline-none w-full cursor-pointer rounded-lg border-dotted border-2 border-blue-500 hover:border-blue-400 bg-white text-blue-500 hover:text-blue-400 py-1"
          @click="adicionar"
        >
          <svg
            class="h-6 w-6"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M12 6v6m0 0v6m0-6h6m-6 0H6"
            />
          </svg>
        </button>
      </div>
      <div class="p-4">
        <div class="w-full text-center text-lg font-medium text-blue-400 mb-2">
          Eletroduto
        </div>
        <div class="flex items-center mb-4">
          <div class="text-base font-normal text-gray-700 mr-2">
            &#8960; utilizado:
          </div>
          <select
            v-model="bitola"
            autocomplete="off"
            class="py-2 px-3 border border-gray-300 bg-white rounded-md focus:outline-none focus:ring-blue-400 focus:border-blue-400 sm:text-sm"
          >
            <option value="-1">Diâmetro mínimo</option>
            <option
              v-for="(item, index) in listaEletrodutos"
              :key="item.diametroNominal"
              :value="index"
              v-html="item.diametroNominal + '&#34;'"
            ></option>
          </select>
        </div>
        <div class="grid grid-cols-3 gap-4">
          <div>
            <div class="rounded-lg bg-gray-100 p-2">
              <span class="font-medium text-xs lg:text-sm text-gray-700"
                >% máximo</span
              >
              <div
                class="w-full text-center font-semibold text-3xl md:text-4xl lg:text-5xl py-3"
              >
                {{ pMaximo + "%" }}
              </div>
            </div>
          </div>
          <div>
            <div class="rounded-lg bg-gray-100 p-2">
              <span class="font-medium text-xs lg:text-sm text-gray-700"
                >% ocupado</span
              >
              <div
                class="w-full text-center font-semibold text-3xl md:text-4xl lg:text-5xl py-3"
                :class="[
                  pOcupado <= pMaximo ? 'text-green-400' : 'text-red-400',
                ]"
                v-html="
                  dMinimo == '-' && areaOcupada > 0
                    ? '&#128552;'
                    : pOcupado + '%'
                "
              ></div>
            </div>
          </div>
          <div>
            <div class="rounded-lg bg-gray-100 p-2">
              <span class="font-medium text-xs lg:text-sm text-gray-700">
                &#8960; mínimo
              </span>
              <div
                class="w-full text-center font-semibold text-3xl md:text-4xl lg:text-5xl py-3"
                v-html="
                  dMinimo == '-'
                    ? areaOcupada > 0
                      ? '&#127757;'
                      : '-&#34;'
                    : listaEletrodutos[dMinimo].diametroNominal + '&#34;'
                "
              ></div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div
      class="fixed inset-x-0 bottom-0 text-center text-xs lg:text-sm text-gray-500 bg-gray-100 p-1"
    >
      v. 0.1.0
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function () {
    return {
      listaEletrodutos: [
        // 1/2''
        {
          diametroNominal: "&#189;",
          diametroInterno: 16.4,
        },
        // 3/4''
        {
          diametroNominal: "&#190;",
          diametroInterno: 21.3,
        },
        // 1''
        {
          diametroNominal: "1",
          diametroInterno: 27.5,
        },
        // 1 1/4''
        {
          diametroNominal: "1&#188;",
          diametroInterno: 36.1,
        },
        // 1 1/2''
        {
          diametroNominal: "1&#189;",
          diametroInterno: 41.4,
        },
        // 2''
        {
          diametroNominal: "2",
          diametroInterno: 52.8,
        },
        // 2 1/2''
        {
          diametroNominal: "2&#189;",
          diametroInterno: 67.1,
        },
        // 3''
        {
          diametroNominal: "3",
          diametroInterno: 79.6,
        },
        // 4''
        {
          diametroNominal: "4",
          diametroInterno: 103.1,
        },
      ],
      listaCabos: [
        //PVC - PRYSMIAN SINTENAX FLEX 0,6/1kV
        {
          formacao: "PVC 1 x 1,5",
          diametroExterno: 5,
        },
        {
          formacao: "PVC 1 x 2,5",
          diametroExterno: 5.4,
        },
        {
          formacao: "PVC 1 x 4",
          diametroExterno: 6.5,
        },
        {
          formacao: "PVC 1 x 6",
          diametroExterno: 7.1,
        },
        {
          formacao: "PVC 1 x 10",
          diametroExterno: 8,
        },
        {
          formacao: "PVC 1 x 16",
          diametroExterno: 9.6,
        },
        {
          formacao: "PVC 1 x 25",
          diametroExterno: 11,
        },
        {
          formacao: "PVC 1 x 35",
          diametroExterno: 13.1,
        },
        {
          formacao: "PVC 1 x 50",
          diametroExterno: 14.7,
        },
        {
          formacao: "PVC 1 x 70",
          diametroExterno: 16.4,
        },
        {
          formacao: "PVC 1 x 95",
          diametroExterno: 18.7,
        },
        {
          formacao: "PVC 1 x 120",
          diametroExterno: 21.1,
        },
        {
          formacao: "PVC 1 x 150",
          diametroExterno: 22.7,
        },
        {
          formacao: "PVC 1 x 185",
          diametroExterno: 24.8,
        },
        {
          formacao: "PVC 1 x 240",
          diametroExterno: 29.1,
        },
        {
          formacao: "PVC 2 x 1,5",
          diametroExterno: 8.6,
        },
        {
          formacao: "PVC 2 x 2,5",
          diametroExterno: 9.7,
        },
        {
          formacao: "PVC 2 x 4",
          diametroExterno: 11.5,
        },
        {
          formacao: "PVC 2 x 6",
          diametroExterno: 12.8,
        },
        {
          formacao: "PVC 2 x 10",
          diametroExterno: 14.7,
        },
        {
          formacao: "PVC 2 x 16",
          diametroExterno: 18.3,
        },
        {
          formacao: "PVC 2 x 25",
          diametroExterno: 20.7,
        },
        {
          formacao: "PVC 2 x 35",
          diametroExterno: 26.3,
        },
        {
          formacao: "PVC 3 x 1,5",
          diametroExterno: 9.3,
        },
        {
          formacao: "PVC 3 x 2,5",
          diametroExterno: 10.2,
        },
        {
          formacao: "PVC 3 x 4",
          diametroExterno: 12.4,
        },
        {
          formacao: "PVC 3 x 6",
          diametroExterno: 13.6,
        },
        {
          formacao: "PVC 3 x 10",
          diametroExterno: 15.8,
        },
        {
          formacao: "PVC 3 x 16",
          diametroExterno: 19.4,
        },
        {
          formacao: "PVC 3 x 25",
          diametroExterno: 22,
        },
        {
          formacao: "PVC 3 x 35",
          diametroExterno: 27.4,
        },
        {
          formacao: "PVC 4 x 1,5",
          diametroExterno: 10.4,
        },
        {
          formacao: "PVC 4 x 2,5",
          diametroExterno: 11.5,
        },
        {
          formacao: "PVC 4 x 4",
          diametroExterno: 14.1,
        },
        {
          formacao: "PVC 4 x 6",
          diametroExterno: 15.4,
        },
        {
          formacao: "PVC 4 x 10",
          diametroExterno: 18.4,
        },
        {
          formacao: "PVC 4 x 16",
          diametroExterno: 22.9,
        },
        {
          formacao: "PVC 4 x 25",
          diametroExterno: 25.9,
        },
        {
          formacao: "PVC 4 x 35",
          diametroExterno: 31,
        },
        //EPR - PRYSMIAN AFUMEX FLEX 0,6/1kV
        {
          formacao: "HEPR 1 x 1,5",
          diametroExterno: 4.8,
        },
        {
          formacao: "HEPR 1 x 2,5",
          diametroExterno: 5.2,
        },
        {
          formacao: "HEPR 1 x 4",
          diametroExterno: 5.7,
        },
        {
          formacao: "HEPR 1 x 6",
          diametroExterno: 6.2,
        },
        {
          formacao: "HEPR 1 x 10",
          diametroExterno: 7.4,
        },
        {
          formacao: "HEPR 1 x 16",
          diametroExterno: 9,
        },
        {
          formacao: "HEPR 1 x 25",
          diametroExterno: 10.3,
        },
        {
          formacao: "HEPR 1 x 35",
          diametroExterno: 12.4,
        },
        {
          formacao: "HEPR 1 x 50",
          diametroExterno: 13.8,
        },
        {
          formacao: "HEPR 1 x 70",
          diametroExterno: 15.7,
        },
        {
          formacao: "HEPR 1 x 95",
          diametroExterno: 17.5,
        },
        {
          formacao: "HEPR 1 x 120",
          diametroExterno: 19.2,
        },
        {
          formacao: "HEPR 1 x 150",
          diametroExterno: 21.7,
        },
        {
          formacao: "HEPR 1 x 185",
          diametroExterno: 23.6,
        },
        {
          formacao: "HEPR 1 x 240",
          diametroExterno: 26.6,
        },
        {
          formacao: "HEPR 1 x 300",
          diametroExterno: 30.6,
        },
        {
          formacao: "HEPR 2 x 1,5",
          diametroExterno: 8.7,
        },
        {
          formacao: "HEPR 2 x 2,5",
          diametroExterno: 9.6,
        },
        {
          formacao: "HEPR 2 x 4",
          diametroExterno: 10.6,
        },
        {
          formacao: "HEPR 2 x 6",
          diametroExterno: 11.7,
        },
        {
          formacao: "HEPR 2 x 10",
          diametroExterno: 14.4,
        },
        {
          formacao: "HEPR 2 x 16",
          diametroExterno: 17.5,
        },
        {
          formacao: "HEPR 2 x 25",
          diametroExterno: 19.7,
        },
        {
          formacao: "HEPR 2 x 35",
          diametroExterno: 23.8,
        },
        {
          formacao: "HEPR 3 x 1,5",
          diametroExterno: 9.2,
        },
        {
          formacao: "HEPR 3 x 2,5",
          diametroExterno: 10.1,
        },
        {
          formacao: "HEPR 3 x 4",
          diametroExterno: 11.2,
        },
        {
          formacao: "HEPR 3 x 6",
          diametroExterno: 12.8,
        },
        {
          formacao: "HEPR 3 x 10",
          diametroExterno: 15.2,
        },
        {
          formacao: "HEPR 3 x 16",
          diametroExterno: 18.6,
        },
        {
          formacao: "HEPR 3 x 25",
          diametroExterno: 21,
        },
        {
          formacao: "HEPR 3 x 35",
          diametroExterno: 25.5,
        },
        {
          formacao: "HEPR 4 x 1,5",
          diametroExterno: 10,
        },
        {
          formacao: "HEPR 4 x 2,5",
          diametroExterno: 11,
        },
        {
          formacao: "HEPR 4 x 4",
          diametroExterno: 12.2,
        },
        {
          formacao: "HEPR 4 x 6",
          diametroExterno: 13.9,
        },
        {
          formacao: "HEPR 4 x 10",
          diametroExterno: 16.8,
        },
        {
          formacao: "HEPR 4 x 16",
          diametroExterno: 21.6,
        },
        {
          formacao: "HEPR 4 x 25",
          diametroExterno: 24.9,
        },
        {
          formacao: "HEPR 4 x 35",
          diametroExterno: 30.5,
        },
      ],
      cabos: [],
      bitola: -1,
    };
  },
  computed: {
    nCabos: function () {
      return this.cabos.length
        ? this.cabos.reduce(function (num, cabo) {
            return num + parseInt(cabo.qtde);
          }, 0)
        : 0;
    },
    pMaximo: function () {
      return this.nCabos > 2
        ? 40
        : this.nCabos > 1
        ? 31
        : this.nCabos > 0
        ? 53
        : 0;
    },
    areaOcupada: function () {
      const listaCabos = this.listaCabos;
      return this.nCabos
        ? this.cabos.reduce(function (area, cabo) {
            return (
              area +
              (cabo.index == -1
                ? 0
                : cabo.qtde *
                  Math.pow(listaCabos[cabo.index].diametroExterno, 2))
            );
          }, 0)
        : 0;
    },
    pOcupado: function () {
      const areaOcupada = this.areaOcupada;
      var areaEletroduto =
        this.dMinimo !== "-" && areaOcupada
          ? Math.pow(
              this.listaEletrodutos[
                this.bitola > -1 ? this.bitola : this.dMinimo
              ].diametroInterno,
              2
            )
          : 1;
      return (
        Math.round(
          ((100 * areaOcupada) / areaEletroduto + Number.EPSILON) * 10
        ) / 10
      );
    },
    dMinimo: function () {
      const areaOcupada = this.areaOcupada;
      const pMaximo = this.pMaximo;
      var indexEletroduto = -1;
      this.listaEletrodutos.forEach(function (eletroduto, index) {
        if (
          indexEletroduto == -1 &&
          areaOcupada <
            (Math.pow(eletroduto.diametroInterno, 2) * pMaximo) / 100
        ) {
          indexEletroduto = index;
        }
      });
      return this.nCabos
        ? indexEletroduto == -1
          ? "-"
          : indexEletroduto
        : "-";
    },
  },
  methods: {
    adicionar: function () {
      this.cabos.push({
        index: -1,
        diametroExterno: 0,
        qtde: 0,
      });
    },
    excluir: function (index) {
      this.cabos.splice(index, 1);
    },
    limpar: function () {
      this.bitola = -1;
      this.cabos = [];
    },
  },
};
</script>
