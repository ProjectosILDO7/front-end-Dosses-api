<template>
    <div class="container mt-4">

        <topo-page-component :namePage="namePage" :nameButton="nameButto" :pageTopoIcon="pageTopoIcon"
            @buscar-curso-id="cleanForm" />

        <br>

        <div class="form-group col-12 text-start mb-2">
            <span class="text-secondary">Total de categoria:</span> <span class="text-secondary">( {{ totalDeCategorias }}
                )</span>
        </div>

        <form class="d-flex" role="search" v-if="totalDeCategorias >= 3">
            <div class="input-group flex-nowrap">
                <span class="input-group-text" id="addon-wrapping"><svg-icon type="mdi"
                        :path="mdiCardSearch"></svg-icon></span>
                <input class="form-control form-control-sm" v-model="filter" type="search" placeholder="Pesquisar"
                    aria-label="Search">
            </div>
        </form>

        <div class="row" v-if="categorias == ''">
            <div class="form-group col-12 text-danger text-center fw-bold">
                De momento não tens nenhuma categoria cadastrado...
            </div>
        </div>

        <div class="card mt-2 shadow">
            <div class="card-body">


                <table class="table table-sm table-bordered">
                    <thead class="table-dark">
                        <tr>
                            <th>#</th>
                            <th>Categorias registadas</th>
                            <th>Opções</th>
                        </tr>
                    </thead>

                    <tbody>
                        <tr v-for="category in categorias" :key="category.id">
                            <td>{{ category.id }}</td>
                            <td>{{ category.nome_categoria }}</td>
                            <td><span class="text-secondary">
                                    <div class="dropdown">
                                        <button class="btn btn-light" type="button" data-bs-toggle="dropdown"
                                            aria-expanded="false">
                                            <svg-icon type="mdi" :path="path"></svg-icon>
                                        </button>
                                        <ul class="dropdown-menu">
                                            <li><a class="dropdown-item" href="#" data-bs-toggle="modal"
                                                    data-bs-target="#modalEdit"
                                                    @click="updateCategoriaForm(category.id)"><svg-icon type="mdi"
                                                        :path="mdiSquareEditOutline"></svg-icon> Alterar</a></li>
                                            <li><a class="dropdown-item" href="#" data-bs-toggle="modal"
                                                    data-bs-target="#modalDeleteConfirm"
                                                    @click="deleteCategoria(category.id)"><svg-icon type="mdi"
                                                        :path="mdiDeleteForever"></svg-icon> Apagar</a></li>
                                            <li><a class="dropdown-item" href="#" data-bs-toggle="offcanvas"
                                                    data-bs-target="#offcanvasRight" aria-controls="offcanvasRight"
                                                    @click="detalhes(category.id)">
                                                    <svg-icon type="mdi" :path="mdiInformationVariantCircle"></svg-icon> Vêr
                                                    Detalhes
                                                </a></li>
                                        </ul>
                                    </div>
                                </span></td>
                        </tr>
                    </tbody>
                </table>

            </div>

            <!-- Listagem em tabela
                 <div class="card-body">

                <div class="row">

                    <div class="form-group col-12">
                        <div class="table-responsive">

                            <table class="table table-sm table-striped">
                                <thead>
                                    <tr>
                                        <th>Cod.</th>
                                        <th>Cursos</th>
                                        <th>Cobrança</th>
                                        <th>Opções</th>
                                    </tr>
                                </thead>

                                <tbody>
                                    <tr v-for="curso in courses.data" :key="curso.id">
                                        <td>{{ curso.id }}</td>
                                        <td>{{ curso.cursos }}</td>
                                        <td>{{ vueNumberFormat(curso.cobranca, { isInteger: true }) }}</td>
                                        <td>
                                            <button @click="updateCursoForm(curso.id)"
                                                class="btn btn-sm btn-outline-success mr-2" type="button"
                                                data-bs-toggle="modal" data-bs-target="#modalEdit" to="#">
                                                <i class="fa-regular fa-pen-to-square"></i>
                                            </button>
                                            <button class="btn btn-sm btn-outline-danger mr-2" data-bs-toggle="modal"
                                                data-bs-target="#modalDeleteConfirm" @click="deleteCourse(curso.id)">
                                                <i class="fa-solid fa-trash"></i>
                                            </button>
                                        </td>
                                    </tr>
                                </tbody>
                            </table> 

                           

                            <nav aria-label="paginate">
                                <ul class="pagination pagination-sm" v-if="courses.last_page > 1">
                                    <li class="page-item" v-if="courses.current_page != 1">
                                        <a class="page-link" href="#"
                                            @click.prevent="loadingCourse(courses.current_page - 1)">{{ '<<' }}</a>
                                    </li>

                                    <li :class="['page-item', { 'active': courses.current_page == page }]" aria-current="page"
                                        v-for="page in courses.last_page" :key="page.id">
                                        <a class="page-link" href="#" @click.prevent="loadingCourse(page)">{{ page }}</a>
                                    </li>

                                    <li class="page-item" v-if="courses.current_page < courses.last_page">
                                        <a class="page-link" href="#"
                                            @click.prevent="loadingCourse(courses.current_page + 1)">{{ '>>' }}</a>
                                    </li>
                                </ul>
                            </nav>


                        </div>
                    </div>
                </div>
            </div> -->
        </div>
        <br>
        <!-- paginate -->
        <!-- <nav aria-label="paginate mt-4">
            <ul class="pagination pagination-sm" v-if="category.last_page > 1">
                <li class="page-item" v-if="category.current_page != 1">
                    <a class="page-link" href="#" @click.prevent="loadingCourse(courses.current_page - 1)">{{ '<<' }}</a>
                </li>

                <li :class="['page-item', { 'active': category.current_page == page }]" aria-current="page"
                    v-for="page in category.last_page" :key="page.id">
                    <a class="page-link" href="#" @click.prevent="loadingCourse(page)">{{ page }}</a>
                </li>

                <li class="page-item" v-if="category.current_page < category.last_page">
                    <a class="page-link" href="#" @click.prevent="loadingCourse(category.current_page + 1)">{{ '>>'
                    }}</a>
                </li>
            </ul>
        </nav> -->

        <!-- Modal cadastrar -->
        <div class="modal fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1"
            aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel"><i class="fa-solid fa-graduation-cap"></i> Cadastre
                            nova categoria</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="row">
                            <form action="">

                                <div class="form-group col-12 mb-2">
                                    <span class="text-danger small col-12" v-if="erros.nome_categoria">{{
                                        erros.nome_categoria[0] }}</span>
                                    <label for="" class="text-secodary col-12">Nome da categoria</label>
                                    <input type="text" class="form-control form-control-sm"
                                        placeholder="Informe nova categoria" v-model="items.nome_categoria">
                                </div>

                                <!-- <div class="form-group col-12 mb-2">
                                    <span class="text-danger small col-12" v-if="erros.remoneracao">{{ erros.remoneracao[0]
                                    }}</span>
                                    <label for="" class="text-secodary col-12">Valor da remoneração</label>
                                    <input type="text" class="form-control form-control-sm" placeholder="Cobrança"
                                        v-model="items.cobranca"> 
                                    <VueNumberFormat class="form-control form-control-sm" v-model:value="items.remoneracao"
                                        :options="{ precision: 2, prefix: '', suffix: ' ', isInteger: true, acceptNegative: false, masked: false }">
                                    </VueNumberFormat>
                                </div> -->

                            </form>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="submit" @click="cleanForm" class="btn btn-sm btn-secondary" data-bs-dismiss="modal">
                            <span>
                                <i class="fa-solid fa-circle-xmark"></i> Fechar
                            </span>
                        </button>
                        <button @click.prevent="registerCategoria" type="button" class="btn btn-sm btn-success">
                            <span v-if="loading">
                                <i class="fa-regular fa-floppy-disk"></i> Tentando salvar...
                            </span>
                            <span v-else>
                                <i class="fa-regular fa-floppy-disk"></i> Cadastrar
                            </span>
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <!-- End Modal cadastrar -->

        <!-- Modal Edit -->
        <div class="modal fade" id="modalEdit" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1"
            aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel"><i class="fa-solid fa-pen-to-square"></i> Alterar
                            categoria</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="row">
                            <form action="">

                                <div class="form-group col-12 mb-2">
                                    <span class="text-danger small col-12" v-if="erros.nome_categoria">{{
                                        erros.nome_categoria[0] }}</span>
                                    <label for="" class="text-secodary col-12">Nome do curso</label>
                                    <input type="text" class="form-control form-control-sm" placeholder="Informe novo curso"
                                        v-model="items.nome_categoria">
                                </div>
                            </form>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="submit" @click="cleanForm" class="btn btn-sm btn-secondary" data-bs-dismiss="modal">
                            <span>
                                <i class="fa-solid fa-circle-xmark"></i> Fechar
                            </span>
                        </button>
                        <button @click.prevent="updateCategoria(items.id)" type="button" class="btn btn-sm btn-success">
                            <span v-if="loading">
                                <i class="fa-regular fa-floppy-disk"></i> Tentando salvar...
                            </span>
                            <span v-else>
                                <i class="fa-regular fa-floppy-disk"></i> Salvar alteração
                            </span>
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <!-- End Modal Edit -->

        <!-- Modal Confirm Delete -->
        <div class="modal fade" id="modalDeleteConfirm" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1"
            aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel"><i class="fa-solid fa-trash text-danger"></i>
                            Apagar a
                            categoria</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="row">
                            <div class="form-group col-12 text-center">
                                <span class="text-primary">
                                    <h6>Tens a certeza que desejas apagar esta categoria do sistema...?</h6>
                                </span>
                            </div>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="submit" class="btn btn-sm btn-secondary" data-bs-dismiss="modal">
                            <span>
                                <i class="fa-solid fa-circle-xmark"></i> Fechar
                            </span>
                        </button>
                        <button @click.prevent="apagarCategoria(deleteCategoriaId)" type="button"
                            class="btn btn-sm btn-danger">
                            <span v-if="loading">
                                <i class="fa-regular fa-floppy-disk"></i> Apagando a categoria...
                            </span>
                            <span v-else>
                                <i class="fa-regular fa-floppy-disk"></i> Apagar
                            </span>
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <!-- End Modal Confirm Delete -->

        <!-- Înfo Canvas - informações a direita -->
        <div class="offcanvas offcanvas-end" tabindex="-1" id="offcanvasRight" aria-labelledby="offcanvasRightLabel">
            <div class="offcanvas-header">

                <div class="form-group col-9 text-end text-info">
                    <h5 id="offcanvasRightLabel text-center">DETALHES DA CATEGORIA</h5>
                </div>
                <div class="form-group col-3 text-end">
                    <button type="button" class="btn-close text-reset" data-bs-dismiss="offcanvas"
                        aria-label="Close"></button>
                </div>

            </div>
            <div class="offcanvas-body">
                <div class="row">
                    <div class="form-group col-12 text-center">
                        <h1><i class="fa fa-graduation-cap fs-lg text-success"></i></h1>
                    </div>
                </div>

                <hr>

                <div class="row">
                    <div class="form-group col-12 text-center">
                        <div class="card shadow">
                            <h4><span class="text-danger">{{ info.nome_categoria }}</span></h4>
                        </div>
                    </div>


                    <div class="form-group col-12 mt-4">
                        <div class="card">
                            <div class="card-body">
                                <h6>Data de registo:</h6>
                                <p class="text-secondary topoMargin"><span> {{ fomatoData }} </span>
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
                <hr>

            </div>
        </div>

    </div>
</template>

<script>
import { notify } from '@kyvg/vue3-notification'
//import dateFormat from '@vue-formily/date-format';
import moment from 'moment';
import 'moment/locale/pt-br';
import topoPageComponent from '../admin/services/partials/topoPageComponent.vue'
import SvgIcon from '@jamescoyle/vue-icon';
import { mdiDotsVertical, mdiSquareEditOutline, mdiDeleteForever, mdiInformationVariantCircle, mdiCardSearch } from '@mdi/js';

export default {
    name: "Curso-component",

    data() {
        return {
            items: { nome_categoria: '' },
            erros: { nome_categoria: '' },
            deleteCategoriaId: '',
            path: mdiDotsVertical, mdiCardSearch, mdiSquareEditOutline, mdiDeleteForever, mdiInformationVariantCircle,
            filter: '',
            info: [],
            fomatoData: '',
            namePage: 'Categorias',
            nameButto: 'Nova categoria',
            pageTopoIcon: 'fa-solid fa-graduation-cap',
            loading: false,
        }
    },
    created() {
        this.loadingCategorias(1)
    },

    computed: {

        categorias() {
             const c = this.$store.getters.todasCategorias(this.filter)
             console.log(c)
             return c
        },

        totalDeCategorias() {
            return this.$store.getters.todasCategoriasCount
        },

    },

    methods: {

        detalhes(id) {
            this.info = []
            this.$store.dispatch('detalhesCategoria', id)
                .then((response) => {
                    this.info = response.data
                    console.log(moment.parseZone(this.info.created_at).locale('pt-br').format('LLLL')); // pt-br
                    this.fomatoData = moment(this.info.created_at).locale('pt-br').format('LLLL')

                })
                .catch((error) => {
                    notify({
                        title: 'Erro de localização',
                        text: error,
                        type: 'warn'
                    })
                })
        },

        cleanForm() {
            this.items = { nome_categoria: '' },
                this.erros = []
        },

        loadingCategorias() {
            //var store = useStore()
            this.$store.dispatch('loadingCategorias')
        },

        registerCategoria() {

            this.$store.dispatch('createCategoria', this.items)
                .then(() => {
                    notify({
                        title: 'Sucesso',
                        text: "A categoria foi registada com sucesso",
                        type: 'success'
                    })
                    this.loadingCategorias()
                    this.cleanForm()
                })
                .catch((error) => {
                    this.erros = error
                    notify({
                        title: 'Erro',
                        text: "Ocorreu um erro durante o processo de cadastro!",
                        type: 'warn'
                    })

                })
        },
        updateCategoriaForm(id) {
            this.cleanForm()
            this.$store.dispatch('updateFormCategoria', id)
                .then((response) => this.items = response.data.getCategoria)
                .catch(() => {
                    notify({
                        title: 'Não encotrado',
                        text: 'A Categoria que pretendes localizar não existe',
                        type: 'warn'
                    })
                })
        },

        updateCategoria() {
            this.$store.dispatch('updateCategoria', this.items)
                .then(() => {
                    notify({
                        title: 'Sucesso',
                        text: 'A categoria foi actualizada com sucesso..!',
                        type: 'success'
                    })
                    this.loadingCategorias()
                    this.cleanForm()
                })
                .catch((error) => {
                    notify({
                        title: 'Erro',
                        text: 'Não foi possível actualizar esta categoria',
                        type: 'warn'
                    })
                    this.erros = error.response.data.errors
                })
        },

        deleteCategoria(id) {
            this.deleteCategoriaId = id;
        },

        apagarCategoria(id) {
            this.$store.dispatch('apagarCategoria', id)
                .then((response) => {
                    notify({
                        title: 'Sucesso',
                        text: response.data.message,
                        type: 'success'
                    })
                    this.loadingCategorias()
                })
                .catch((error) => {
                    notify({
                        title: 'Erro',
                        text: error.data.erro,
                        type: 'warn'
                    })
                })
        }
    },

    components: {
        topoPageComponent,
        SvgIcon
    }
}
</script>

<style scoped>
.topoMargin {
    margin-top: -3% !important;
}
</style>