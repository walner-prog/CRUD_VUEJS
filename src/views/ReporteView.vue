<template>
    <div class="row">
      <div class="col-lg-4 mb-3">
        <button @click="goToCreateProductView" class="btn btn-success">
          <i class="fa-solid fa-plus"></i> Crear Producto
        </button>
      </div>
      <div class="col-lg-8 offset-lg-2">
        <div class="table-responsive">
          <DataTable :data="products" :columns="columns" class="table table-striped table-bordered display custom-table"
            :options="{ responsive:true, autoWidth:false, dom:'Bfrtip', language:{ search:'Buscar', zeroRecords:'No hay registros para mostrar', info: 'Mostrando del _START_ a _END_ de _TOTAL_ resgistros', infoFiltered: '(Filtrados de _MAX_ registros.)', paginate:{ first:'Primero', previous:'Anterior', next:'Siguiente', last:'Último' } }, buttons:botones }">
           
            <thead class=" mt-4">
              <tr>
                <th>#</th>
                <th>NOMBRE</th>
                <th>DESCRIPCION</th>
                <th>PRECIO</th>
                <th>Categoria</th>
              </tr>
            </thead>
          </DataTable>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import DataTable from 'datatables.net-vue3';
  import DataTableLib from 'datatables.net-bs5'
  import Buttons from 'datatables.net-buttons-bs5'
  import ButtonsHtml5 from 'datatables.net-buttons/js/buttons.html5'
  import print from 'datatables.net-buttons/js/buttons.print'
  import pdfmake from 'pdfmake';
  import pdfFonts from 'pdfmake/build/vfs_fonts';
  pdfmake.vfs = pdfFonts.pdfMake.vfs;
  import 'datatables.net-responsive-bs5';
  import JsZip from 'jszip';
  window.JSZip = JsZip;
  DataTable.use(DataTableLib);
  DataTable.use(pdfmake);
  DataTable.use(ButtonsHtml5);
  export default{
      components: {DataTable},
      data(){
          return {
              products:null,
              columns:[
                  {data:null, render: function(data,type,row,meta)
                  {return `${meta.row+1}`}},
                 // {data:'id'},
                  {data:'name'},
                  {data:'description'},
                  {data: null, render: function(data,type,row,meta)
                  {return ('$'+ new Intl.NumberFormat('es-mx').format(data.price))}},
                  
                  {data:'category'},
              ],
              botones: [
                  {
                      title:'Reporte de productos',
                      extend:'excelHtml5',
                      text: '<i class="fa-solid fa-file-excel"></i> Excel',
                      className:'btn btn-success'
                  },
                  {
                      title:'Reporte de productos',
                      extend:'pdfHtml5',
                      text: '<i class="fa-solid fa-file-pdf"></i> PDF',
                      className:'btn btn-danger'
                  },
                  {
                      title:'Reporte de productos',
                      extend:'print',
                      text: '<i class="fa-solid fa-print"></i> Imprimir',
                      className:'btn btn-dark'
                  },
                  {
                      title:'Reporte de productos',
                      extend:'copy',
                      text: '<i class="fa-solid fa-copy"></i> Copiar texto',
                      className:'btn btn-secondary'
                  }
              ]
          }
      },
      mounted(){
          this.getProducts();
      },
      methods:{
          getProducts(){
              axios.get('https://apiproduct-nveo.onrender.com/products').then(
                  response => (
                      this.products = response.data
                  )
              );
          }
      }
  }
  </script>
  
  <style scoped>
  .custom-table th,
  .custom-table td {
    font-size: 14px;
    padding: 10px;
    text-align: center;
  }
  
  .custom-table th {
    background-color: #f2f2f2;
    color: #333;
  }
  
  .custom-table tbody tr:hover {
    background-color: #f8f9fa;
  }
  
  .tooltip {
    position: relative;
    display: inline-block;
    cursor: pointer;
  }
  
  .tooltip .tooltiptext {
    visibility: hidden;
    width: 200px;
    background-color: #333;
    color: #fff;
    text-align: center;
    border-radius: 6px;
    padding: 5px;
    position: absolute;
    z-index: 1;
    bottom: 125%;
    left: 50%;
    margin-left: -100px;
    opacity: 0;
    transition: opacity 0.3s;
  }
  
  .tooltip:hover .tooltiptext {
    visibility: visible;
    opacity: 1;
  }
  </style>
  