<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="OrdemDeEntrada.aspx.cs" Inherits="Federacao.OrdemDeEntrada" %>

<!DOCTYPE html>
<html lang="pt-br">
<head runat="server">
    <meta charset="UTF-8">
    <title>Ordem de Entrada</title>
    <style>
        .alerta {
            background-color: #FF6666 !important;
        }
        table {
            width: 100%;
            border-collapse: collapse;
        }
        th, td {
            border: 1px solid black;
            padding: 8px;
            text-align: left;
        }
    </style>
</head>
<body>
    <form id="form1" runat="server">
        <h2>Ordem de Entrada</h2>
        <asp:GridView ID="gvOrdemEntrada" runat="server" AutoGenerateColumns="False" CssClass="tabela">
            <Columns>
                <asp:BoundField DataField="Ordem" HeaderText="Ordem" />
                <asp:TemplateField HeaderText="Cavaleiro">
                    <ItemTemplate>
                        <asp:Label ID="lblCavaleiro" runat="server" Text='<%# Eval("Cavaleiro") %>'></asp:Label>
                    </ItemTemplate>
                </asp:TemplateField>
                <asp:BoundField DataField="Cavalo" HeaderText="Cavalo" />
            </Columns>
        </asp:GridView>
    </form>
</body>
</html>


using System;
using System.Collections.Generic;
using System.Data;
using System.Linq;
using System.Web.UI;
using System.Web.UI.WebControls;

namespace Federacao
{
    public partial class OrdemDeEntrada : Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
            if (!IsPostBack)
            {
                CarregarOrdemDeEntrada();
            }
        }

        private void CarregarOrdemDeEntrada()
        {
            // Simulando dados (normalmente, isso viria do banco de dados)
            List<Entrada> lista = new List<Entrada>
            {
                new Entrada { Ordem = 1, Cavaleiro = "João Silva", Cavalo = "Thunder" },
                new Entrada { Ordem = 5, Cavaleiro = "Maria Souza", Cavalo = "Relâmpago" },
                new Entrada { Ordem = 8, Cavaleiro = "João Silva", Cavalo = "Tempestade" },
                new Entrada { Ordem = 15, Cavaleiro = "João Silva", Cavalo = "Flecha" },
                new Entrada { Ordem = 12, Cavaleiro = "Carlos Mendes", Cavalo = "Vento Forte" }
            };

            // Criando um DataTable para popular o GridView
            DataTable dt = new DataTable();
            dt.Columns.Add("Ordem");
            dt.Columns.Add("Cavaleiro");
            dt.Columns.Add("Cavalo");

            foreach (var entrada in lista)
            {
                dt.Rows.Add(entrada.Ordem, entrada.Cavaleiro, entrada.Cavalo);
            }

            gvOrdemEntrada.DataSource = dt;
            gvOrdemEntrada.DataBind();

            // Aplicar a regra de proximidade
            AplicarAlertaCavaleiros(lista);
        }

        private void AplicarAlertaCavaleiros(List<Entrada> lista)
        {
            for (int i = 0; i < lista.Count; i++)
            {
                for (int j = i + 1; j < lista.Count; j++)
                {
                    if (lista[i].Cavaleiro == lista[j].Cavaleiro &&
                        Math.Abs(lista[i].Ordem - lista[j].Ordem) < 10) // Se a diferença for menor que 10 posições
                    {
                        // Procurando os Labels dentro do GridView e aplicando a classe CSS
                        foreach (GridViewRow row in gvOrdemEntrada.Rows)
                        {
                            Label lblCavaleiro = (Label)row.FindControl("lblCavaleiro");
                            if (lblCavaleiro != null && (lblCavaleiro.Text == lista[i].Cavaleiro || lblCavaleiro.Text == lista[j].Cavaleiro))
                            {
                                row.Cells[1].CssClass = "alerta";
                            }
                        }
                    }
                }
            }
        }

        public class Entrada
        {
            public int Ordem { get; set; }
            public string Cavaleiro { get; set; }
            public string Cavalo { get; set; }
        }
    }
}
