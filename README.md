# C-FrameWork 
セレクト側
private void cmbInternName_SelectedIndexChanged(object sender, EventArgs e)
{
    if (cmbInternName.SelectedItem == null) return;

    string selectedName = cmbInternName.SelectedItem.ToString();
    string[] lines = File.ReadAllLines("internData.dat");

    for (int i = 0; i < lines.Length - 3; i += 4) // ←4行1セット
    {
        string name = lines[i];
        string start = lines[i + 1];
        string end = lines[i + 2];

        if (name == selectedName)
        {
            if (DateTime.TryParse(start, out DateTime s))
                dtpStart.Value = s;

            if (DateTime.TryParse(end, out DateTime e1))
                dtpEnd.Value = e1;

            break;
        }
    }
}




コンボ側
private void LoadInternProgramsToCombo()
{
    cmbInternName.Items.Clear();

    string[] lines = File.ReadAllLines("internData.dat");

    for (int i = 0; i < lines.Length - 3; i += 4)
    {
        cmbInternName.Items.Add(lines[i]); // インターン名称のみ追加
    }
}

private void dgvMain_CellValueChanged(object sender, DataGridViewCellEventArgs e)
{
    if (e.RowIndex < 0) return; // ヘッダ行は無視

    if (dgvMain.Columns[e.ColumnIndex].Name == "InternNameColumn")
    {
        DataGridViewRow row = dgvMain.Rows[e.RowIndex];
        string selectedName = row.Cells["InternNameColumn"].Value?.ToString();
        if (string.IsNullOrEmpty(selectedName)) return;

        string[] lines = File.ReadAllLines("internData.dat");

        for (int i = 0; i < lines.Length - 3; i += 4)
        {
            string name = lines[i];
            string start = lines[i + 1];
            string end = lines[i + 2];

            if (name == selectedName)
            {
                if (DateTime.TryParse(start, out DateTime s))
                    row.Cells["StartDateColumn"].Value = s;

                if (DateTime.TryParse(end, out DateTime e1))
                    row.Cells["EndDateColumn"].Value = e1;

                break;
            }
        }
    }
}


dgvMain.CellValueChanged += dgvMain_CellValueChanged;

