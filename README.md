
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Data Pemasukan Sawit dan Pembelian Pupuk</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
</head>

<body>

    <div class="container my-5">
        <h2 class="text-center mb-4">Data Pemasukan Timbangan Sawit dan Pembelian Pupuk</h2>

        <!-- Action Buttons -->
        <div class="mb-3">
            <button class="btn btn-success" onclick="showAddModal()">Tambah Data</button>
            <button class="btn btn-primary" onclick="printData()">Print</button>
        </div>

        <!-- Data Table -->
        <table class="table table-bordered table-striped" id="dataTable">
            <thead class="table-dark">
                <tr>
                    <th>No</th>
                    <th>Tanggal</th>
                    <th>Berat Timbangan Sawit (kg)</th>
                    <th>Jumlah Pupuk yang Dibeli (kg)</th>
                    <th>Total Biaya Pupuk (Rp)</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody id="dataBody">
                <!-- Sample Data Row -->
                <tr>
                    <td>1</td>
                    <td>2024-11-01</td>
                    <td>500</td>
                    <td>100</td>
                    <td>Rp 1.000.000</td>
                    <td>
                        <button class="btn btn-warning btn-sm" onclick="editRow(this)">Edit</button>
                        <button class="btn btn-danger btn-sm" onclick="deleteRow(this)">Hapus</button>
                    </td>
                </tr>
                <!-- Tambahkan data lainnya di sini -->
            </tbody>
        </table>
    </div>

    <!-- Modal Tambah/Edit Data -->
    <div class="modal fade" id="dataModal" tabindex="-1" aria-labelledby="dataModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="dataModalLabel">Tambah/Edit Data</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <form id="dataForm">
                        <div class="mb-3">
                            <label for="tanggal" class="form-label">Tanggal</label>
                            <input type="date" class="form-control" id="tanggal" required>
                        </div>
                        <div class="mb-3">
                            <label for="berat" class="form-label">Berat Timbangan Sawit (kg)</label>
                            <input type="number" class="form-control" id="berat" required>
                        </div>
                        <div class="mb-3">
                            <label for="jumlahPupuk" class="form-label">Jumlah Pupuk yang Dibeli (kg)</label>
                            <input type="number" class="form-control" id="jumlahPupuk" required>
                        </div>
                        <div class="mb-3">
                            <label for="totalBiaya" class="form-label">Total Biaya Pupuk (Rp)</label>
                            <input type="text" class="form-control" id="totalBiaya" required>
                        </div>
                        <input type="hidden" id="editIndex">
                        <button type="submit" class="btn btn-primary">Simpan</button>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <!-- Bootstrap and JavaScript -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
    <script>
        let editMode = false;

        // Show the Add Data Modal
        function showAddModal() {
            editMode = false;
            document.getElementById("dataForm").reset();
            document.getElementById("dataModalLabel").innerText = "Tambah Data";
            const modal = new bootstrap.Modal(document.getElementById("dataModal"));
            modal.show();
        }

        // Edit Row
        function editRow(button) {
            editMode = true;
            const row = button.closest("tr");
            const cells = row.querySelectorAll("td");

            document.getElementById("tanggal").value = cells[1].innerText;
            document.getElementById("berat").value = cells[2].innerText;
            document.getElementById("jumlahPupuk").value = cells[3].innerText;
            document.getElementById("totalBiaya").value = cells[4].innerText;
            document.getElementById("editIndex").value = row.rowIndex - 1;
            document.getElementById("dataModalLabel").innerText = "Edit Data";

            const modal = new bootstrap.Modal(document.getElementById("dataModal"));
            modal.show();
        }

        // Delete Row
        function deleteRow(button) {
            if (confirm("Apakah Anda yakin ingin menghapus data ini?")) {
                const row = button.closest("tr");
                row.remove();
                updateRowNumbers();
            }
        }

        // Save Data (Add or Edit)
        document.getElementById("dataForm").addEventListener("submit", function(event) {
            event.preventDefault();

            const tanggal = document.getElementById("tanggal").value;
            const berat = document.getElementById("berat").value;
            const jumlahPupuk = document.getElementById("jumlahPupuk").value;
            const totalBiaya = document.getElementById("totalBiaya").value;

            if (editMode) {
                const index = document.getElementById("editIndex").value;
                const row = document.getElementById("dataBody").rows[index];
                row.cells[1].innerText = tanggal;
                row.cells[2].innerText = berat;
                row.cells[3].innerText = jumlahPupuk;
                row.cells[4].innerText = totalBiaya;
            } else {
                const newRow = document.createElement("tr");
                newRow.innerHTML = `
                    <td></td>
                    <td>${tanggal}</td>
                    <td>${berat}</td>
                    <td>${jumlahPupuk}</td>
                    <td>${totalBiaya}</td>
                    <td>
                        <button class="btn btn-warning btn-sm" onclick="editRow(this)">Edit</button>
                        <button class="btn btn-danger btn-sm" onclick="deleteRow(this)">Hapus</button>
                    </td>
                `;
                document.getElementById("dataBody").appendChild(newRow);
            }

            updateRowNumbers();
            bootstrap.Modal.getInstance(document.getElementById("dataModal")).hide();
        });

        // Update Row Numbers
        function updateRowNumbers() {
            const rows = document.querySelectorAll("#dataBody tr");
            rows.forEach((row, index) => {
                row.cells[0].innerText = index + 1;
            });
        }

        // Print Data
        function printData() {
            window.print();
        }
    </script>
</body>

</html>
