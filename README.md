pdfmake build : 0.1.39

fonts:
	1. 微軟雅黑
	
=== usage ===

	import pdfMake from "pdfmake-with-chinese-fonts/pdfmake";
	import pdfFonts from "pdfmake-with-chinese-fonts/vfs_fonts";

	pdfMake.vfs = pdfFonts.pdfMake.vfs;

	pdfMake.fonts = {
 	 Roboto: {
    	 normal: 'Roboto-Regular.ttf',
    	 bold: 'Roboto-Medium.ttf',
     	italics: 'Roboto-Italic.ttf',
     	bolditalics: 'Roboto-MediumItalic.ttf'
	 },
	 msyh: { 
   	  normal: 'msyh.ttf',
   	  bold: 'msyh.ttf',
   	  italics: 'msyh.ttf',
    	 bolditalics: 'msyh.ttf'
   	  }
 	};
 
	defaultStyle: {font: 'msyh' },`

=== example 解決 :  Datatable pdf 中文會顯示☒問題 ===

in "/lib/pdfmake-with-chinese-fonts" 放入 : 
	1. pdfmake.min.js
	2. pdfmake.min.js.map
	3. vfs_fonts.js

in index.html import : 

    <script src="/lib/pdfmake-with-chinese-fonts/pdfmake.min.js"></script>
    <script src="/lib/pdfmake-with-chinese-fonts/vfs_fonts.js"></script>
    <script>
        pdfMake.vfs = pdfMake.vfs;

        pdfMake.fonts = {
            Roboto: {
                normal: 'Roboto-Regular.ttf',
                bold: 'Roboto-Medium.ttf',
                italics: 'Roboto-Italic.ttf',
                bolditalics: 'Roboto-MediumItalic.ttf'
            },
            msyh: {
                normal: 'msyh.ttf',
                bold: 'msyh.ttf',
                italics: 'msyh.ttf',
                bolditalics: 'msyh.ttf'
            }
        };
    </script>

in datatable  :
This is a server side example (for my convenience to copy in the future), you only need to refer to the pdfHtml5 in buttons: []

    $(document).ready(function() {
        // Initialization DataTable
        var table = $('#myTable').DataTable({
            dom: "<'row'<'col-sm-12 col-md-4'l><'col-sm-12 col-md-4'B><'col-sm-12 col-md-4'f>>" +
                "<'row p-2'<'col-sm-12'tr>>" +
                "<'row p-2'<'col-sm-12 col-md-4'i><'col-sm-12 col-md-8'p>>",
            language: {
                url: '//cdn.datatables.net/plug-ins/2.0.2/i18n/zh-HANT.json'
            },
            ajax: {
                url: "ServerProcessing.php",
                data: function(d) {
                    d.v1 = $('#v1').val();
                    d.v2 = $('#v2').val();
                }
            },
            lengthMenu: [
                [10, 50, 100, 500, 1000, -1],
                [10, 50, 100, 500, 1000, "All"]
            ],
            processing: true,
            serverSide: true,
            buttons: [
                'copy',
                {
                    extend: 'csv',
                    charset: 'UTF-8',
                    bom: true
                },
                {
                    extend: 'pdfHtml5',
                    customize: function(doc) {
                        // 設置自定義字體
                        doc.defaultStyle.font = 'msyh'; // 使用字體的名稱
                    }
                },
                'excel'
            ]
        });
    });
