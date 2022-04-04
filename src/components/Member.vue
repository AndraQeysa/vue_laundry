<template>
  <div>
    <main>
      <div class="container-fluid px-4">
        <h1 class="mt-4">Data Member</h1>
        <div class="card mb-4">
          <div class="card-body">
            <a class="btn btn-success" v-b-modal.modal_member @click="Add()"
              >Tambah Member</a
            >
            <table class="table">
              <tr>
                <td>ID Member</td>
                <td>Nama</td>
                <td>Jenis Kelamin</td>
                <td>Telepon</td>
                <td>Alamat</td>
                <td>Aksi</td>
              </tr>
              <tr v-for="mem in member" :key="mem">
                <td>{{ mem.id_member }}</td>
                <td>{{ mem.nama }}</td>
                <td>{{ mem.jenis_kelamin }}</td>
                <td>{{ mem.tlp }}</td>
                <td>{{ mem.alamat }}</td>
                <td>
                  <a
                    v-b-modal.modal_member
                    href="#"
                    class="btn btn-info"
                    @click="Edit(mem)"
                    >Ubah</a
                  >
                  <a
                    href="#"
                    class="btn btn-danger"
                    @click="Delete(mem.id_member)"
                    >Hapus</a
                  >
                </td>
              </tr>
            </table>
          </div>
        </div>
      </div>
    </main>

    <b-modal
      id="modal_member"
      ref="modal"
      title="Form Member"
      size="md"
      @ok="Save"
    >
      <form>
        <div class="form-floating mb-3 mb-md-0">
          <input
            v-model="nama"
            placeholder="Masukkan Nama Member"
            id="inputNama"
            class="form-control"
            type="text"
          />
          <label for="inputNama">Nama</label>
        </div>
        <div class="form-floating mb-3 mb-md-0">
          <input
            v-model="tlp"
            placeholder="Masukkan Nomor Telepon Member"
            id="inputTlp"
            class="form-control"
            type="text"
          />
          <label for="inputTlp">Telepon</label>
        </div>
        <div class="form-floating mb-3 mb-md-0">
          <select v-model="jk" class="form-control">
            <option selected value="">-- Pilih Jenis Kelamin --</option>
            <option value="l">Laki-Laki</option>
            <option value="p">Perempuan</option>
          </select>

          <label for="inputJk">Jenis Kelamin</label>
        </div>
        <div class="form-floating mb-3 mb-md-0">
          <input
            v-model="alamat"
            placeholder="Masukkan Alamat Member"
            id="inputAlamat"
            class="form-control"
            type="text"
          />
          <label for="inputAlamat">Alamat</label>
        </div>
      </form>
    </b-modal>
  </div>
</template>
<script>
module.exports = {
  data: function () {
    return {
      id_member: "",
      nama: "",
      jk: "",
      tlp: "",
      alamat: "",
      member: [],
      action: "",
    };
  },
  methods: {
    getData: function () {
      let config = {
        headers: {
          Authorization: "Bearer " + this.$cookies.get("Authorization"),
        },
      };

      axios.get(base_url + "/member", config).then((response) => {
        console.log(response);
        if (response.data.success == true) {
          this.member = response.data.data.member;
        } else {
          location.reload();
        }
      });
    },
    Add: function () {
      this.action = "insert";
      this.id_member = "";
      this.nama = "";
      this.jk = "";
      this.tlp = "";
      this.alamat = "";
    },
    Edit: function (item) {
      this.action = "update";
      this.id_member = item.id_member;
      this.nama = item.nama;
      this.jk = item.jenis_kelamin;
      this.tlp = item.tlp;
      this.alamat = item.alamat;
    },
    Save: function () {
      let config = {
        headers: {
          Authorization: "Bearer " + this.$cookies.get("Authorization"),
        },
      };

      let form = {
        nama: this.nama,
        alamat: this.alamat,
        jenis_kelamin: this.jk,
        tlp: this.tlp,
      };

      //logika method post/get (insert /update)
      if (this.action == "insert") {
        axios
          .post(base_url + "/member", form, config)
          .then((response) => {
            Swal.fire(response.data.message);
          })
          .catch((error) => {
            console.log(error);
          });
      } else {
        //update
        axios
          .put(base_url + "/member/" + this.id_member, form, config)
          .then((response) => {
            Swal.fire(response.data.message);
          })
          .catch((error) => {
            console.log(error);
          });
      }

      this.getData();
    },
    Delete: function (id) {
      Swal.fire({
        title: "Apakah Anda Yakin Ingin Menghapus Data Ini?",
        text: "Data akan dihapus secara permanen dan tidak dapat dikembalikan",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Hapus",
      }).then((result) => {
        if (result.isConfirmed) {
          let config = {
            headers: {
              Authorization: "Bearer " + this.$cookies.get("Authorization"),
            },
          };

          axios.delete(base_url + "/member/" + id, config).then((response) => {
            Swal.fire(response.data.message);
          });

          this.getData();
        }
      });
    },
  },
  mounted() {
    this.getData();
  },
};
</script>
