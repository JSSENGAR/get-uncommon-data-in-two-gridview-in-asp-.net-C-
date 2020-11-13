# get-uncommon-data-in-two-gridview-in-asp-.net-C-
get uncommon data in two gridview in asp .net C#


1. Fill view state

            GridView8.DataSource = dt;
            GridView8.DataBind();
            ViewState["Grid2Data"] = dt;
2. Function to get uncommon data in two gridview in asp .net c#


 protected void GetUncommonData(object sender, EventArgs e)
        {
            DataTable dt1 = (DataTable)ViewState["Grid1Data"];
            DataTable dt2 = (DataTable)ViewState["Grid2Data"];
            var qry1 = dt1.AsEnumerable().Select(a => new { Id = a[DropDownList4.SelectedItem.Text].ToString() });
            var qry2 = dt2.AsEnumerable().Select(b => new { Id = b[DropDownList4.SelectedItem.Text].ToString() });

            var exceptAB = qry1.Except(qry2);

            var exceptBA = qry2.Except(qry1);

            var mismatchDataAB = (from a in dt1.AsEnumerable()
                                  join ab in exceptAB on a[DropDownList4.SelectedItem.Text].ToString() equals ab.Id
                                  select a);

            var mismatchDataBA = (from a in dt2.AsEnumerable()
                                  join ab in exceptBA on a[DropDownList4.SelectedItem.Text].ToString() equals ab.Id
                                  select a);
            var unCommon = mismatchDataAB.Union(mismatchDataBA);
            if (unCommon.ToArray().Length > 0)
            {
                GridView9.DataSource = unCommon.CopyToDataTable();
                GridView9.DataBind();
            }
            Labeloffucc.Text = GridView9.Rows.Count.ToString();
            ucontooff();
            TabContainer1.ActiveTabIndex = 3;
        }

3. Call Function

<asp:Button Text="Get Uncommon Data" runat="server" OnClick="GetUncommonData" />
