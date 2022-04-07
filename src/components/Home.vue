<template>
  <div class="container-fluid py-4">
    <div class="row">
      <div class="col-xl-3 col-sm-6 mb-xl-0 mb-4">
        <div class="card">
          <div class="card-body p-3 bg-success bg-gradient">
            <div class="row">
              <div class="col-8">
                <div class="numbers">
                  <p class="text-sm mb-0 text-uppercase fw-bolder text-white">
                    Member
                  </p>
                  <br />

                  <h5 class="fw-bolder text-white">{{ member.length }}</h5>
                </div>
              </div>
              <div class="col-4 text-end">
                <div
                  class="
                    icon icon-shape
                    bg-gradient-primary
                    shadow-primary
                    text-center
                    rounded-circle
                  "
                >
                  <i
                    class="ni ni-money-coins text-lg opacity-10"
                    aria-hidden="true"
                  ></i>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-xl-3 col-sm-6 mb-xl-0 mb-4">
        <div class="card">
          <div class="card-body p-3 bg-success bg-gradient">
            <div class="row">
              <div class="col-8">
                <div class="numbers">
                  <p class="text-sm mb-0 text-uppercase fw-bolder text-white">
                    Outlet
                  </p>
                  <br />

                  <h5 class="fw-bolder text-white">{{ outlet.length }}</h5>
                </div>
              </div>
              <div class="col-4 text-end">
                <div
                  class="
                    icon icon-shape
                    bg-gradient-primary
                    shadow-primary
                    text-center
                    rounded-circle
                  "
                >
                  <i
                    class="ni ni-money-coins text-lg opacity-10"
                    aria-hidden="true"
                  ></i>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-xl-3 col-sm-6 mb-xl-0 mb-4">
        <div class="card">
          <div class="card-body p-3 bg-success bg-gradient">
            <div class="row">
              <div class="col-8">
                <div class="numbers">
                  <p class="text-sm mb-0 text-uppercase fw-bolder text-white">
                    Transaksi
                  </p>
                  <br />

                  <h5 class="fw-bolder text-white">{{ transaksi.length }}</h5>
                </div>
              </div>
              <div class="col-4 text-end">
                <div
                  class="
                    icon icon-shape
                    bg-gradient-primary
                    shadow-primary
                    text-center
                    rounded-circle
                  "
                >
                  <i
                    class="ni ni-money-coins text-lg opacity-10"
                    aria-hidden="true"
                  ></i>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-xl-3 col-sm-6 mb-xl-0 mb-4">
        <div class="card">
          <div class="card-body p-3 bg-success bg-gradient">
            <div class="row">
              <div class="col-8">
                <div class="numbers">
                  <p class="text-sm mb-0 text-uppercase fw-bolder text-white">
                    User
                  </p>
                  <br />

                  <h5 class="fw-bolder text-white">{{ paket.length }}</h5>
                </div>
              </div>
              <div class="col-4 text-end">
                <div
                  class="
                    icon icon-shape
                    bg-gradient-primary
                    shadow-primary
                    text-center
                    rounded-circle
                  "
                >
                  <i
                    class="ni ni-money-coins text-lg opacity-10"
                    aria-hidden="true"
                  ></i>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
module.exports = {
  data: function () {
    return {
      member: [],
      outlet: [],
      paket: [],
      transaksi: [],
    };
  },
  methods: {
    getDataMember: function () {
      let config = {
        headers: {
          Authorization: "Bearer " + this.$cookies.get("Authorization"),
        },
      };
      axios.get(base_url + "/member", config).then((response) => {
        console.log(response);
        if (response.data.success == true) {
          this.member = response.data.data.member;
        }
      });
    },

    getDataOutlet: function () {
      let config = {
        headers: {
          Authorization: "Bearer " + this.$cookies.get("Authorization"),
        },
      };
      axios.get(base_url + "/outlet", config).then((response) => {
        console.log(response);
        if (response.data.success == true) {
          this.outlet = response.data.data.outlet;
        }
      });
    },

    getDataPaket: function () {
      let config = {
        headers: {
          Authorization: "Bearer " + this.$cookies.get("Authorization"),
        },
      };
      axios.get(base_url + "/paket", config).then((response) => {
        console.log(response);
        if (response.data.success == true) {
          this.paket = response.data.data.paket;
        }
      });
    },

    getDataTransaksi: function () {
      let conf = {
        headers: {
          Authorization: "Bearer " + this.$cookies.get("Authorization"),
        },
      };
      let form = {
        tahun: this.tahun,
        bulan: this.bulan,
        tgl: this.tgl,
      };
      axios
        .post(base_url + "/transaksi/report", form, conf)
        .then((response) => {
          this.transaksi = response.data.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  mounted() {
    this.getDataMember();
    this.getDataOutlet();
    this.getDataPaket();
    this.getDataTransaksi();
  },
};
</script>
