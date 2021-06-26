<template>
  <b-container fluid>
    <b-button
      variant="outline-primary"
      v-b-modal.modal-add
    >
      {{ msg }}
    </b-button>
    <b-card class="m-auto mt-3">
      <b-table responsive striped hover :items="items" :fields="fields">

        <template #cell(actions)="data">
          <b-button
            size="sm"
            variant="success"
            v-b-modal.modal-edit
            @click="setPatient(data)"
            style="margin-right: 10px;"
          >
            Edit
          </b-button>

          <b-button
            size="sm"
            variant="danger"
            @click="deletePatient(data)"
          >
            Delete
          </b-button>
        </template>

      </b-table>
    </b-card>

    <b-modal id="modal-add" title="Add Data Patient" hide-footer centered>
      <b-form>

        <b-form-group label="Name">
          <b-form-input
            class="mb-3"
            v-model="form.patient_name"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group label="Sex">
          <b-form-select
            class="mb-3 form-select"
            v-model="form.patient_sex"
            :options="gender"
            required
          ></b-form-select>
        </b-form-group>

        <b-form-group label="Religion">
          <b-form-input
            class="mb-3"
            v-model="form.patient_religion"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group label="Phone">
          <b-form-input
            class="mb-3"
            v-model="form.patient_phone"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group label="Address">
          <b-form-input
            class="mb-3"
            v-model="form.patient_address"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group label="NIK">
          <b-form-input
            class="mb-3"
            v-model="form.patient_nik"
            required
          ></b-form-input>
        </b-form-group>

        <b-button
          type="submit"
          variant="primary"
          style="margin: 0 auto; display: block;"
          @click="postPatient"
          >
          Submit
        </b-button>

      </b-form>
    </b-modal>

    <b-modal id="modal-edit" title="Edit Data Patient" hide-footer centered>
      <b-form>

        <b-form-group label="Name" label-cols-sm="3">
          <b-form-input
            v-model="form.patient_name"
            required
          >
          </b-form-input>
        </b-form-group>

        <b-form-group label="Sex" label-cols-sm="3">
          <b-form-input
            v-model="form.patient_sex"
            required
          >
          </b-form-input>
        </b-form-group>

        <b-form-group label="Religion" label-cols-sm="3">
          <b-form-input
            v-model="form.patient_religion"
            required
          >
          </b-form-input>
        </b-form-group>

        <b-form-group label="Phone" label-cols-sm="3">
          <b-form-input
            v-model="form.patient_phone"
            required
          >
          </b-form-input>
        </b-form-group>

        <b-form-group label="Address" label-cols-sm="3">
          <b-form-input
            v-model="form.patient_address"
            required
          >
          </b-form-input>
        </b-form-group>

        <b-form-group label="NIK" label-cols-sm="3">
          <b-form-input
            v-model="form.patient_nik"
            required
          >
          </b-form-input>
        </b-form-group>

        <b-button
          type="button"
          variant="primary"
          class="mt-3"
          style="margin: 0 auto; display: block;"
          @click="updatePatient"
          >
          Update
        </b-button>

      </b-form>
    </b-modal>
  </b-container>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Create',
  props: {
    msg: String,
  },
  data() {
    return {
      form: {
        patient_name: '',
        patient_sex: '',
        patient_religion: '',
        patient_phone: '',
        patient_address: '',
        patient_nik: '',
      },
      fields: [
        {
          key: 'patient_id',
          label: 'ID',
          sortable: true,
        },
        {
          key: 'patient_name',
          label: 'Name',
          sortable: true,
        },
        {
          key: 'patient_sex',
          label: 'Gender',
          sortable: true,
        },
        {
          key: 'patient_religion',
          label: 'Religion',
          sortable: true,
        },
        {
          key: 'patient_phone',
          label: 'Phone',
          sortable: false,
        },
        {
          key: 'patient_address',
          label: 'Address',
          sortable: false,
        },
        {
          key: 'patient_nik',
          label: 'NIK',
          sortable: false,
        },
        {
          key: 'actions',
          sortable: false,
        },
      ],
      items: [],
      gender: ['Laki-laki', 'Perempuan'],
      infoModal: {
        id: 'info-modal',
        title: '',
        content: '',
      },
    };
  },
  created() {
    this.getPatients();
  },
  methods: {
    async getPatients() {
      try {
        const response = await axios.get('http://localhost:3001/users/');
        // eslint-disable-next-line array-callback-return
        response.data.data.map((item) => {
          const setPatient = {
            patient_id: item.patient_id,
            patient_name: item.patient_name,
            patient_sex: item.patient_sex,
            patient_religion: item.patient_religion,
            patient_phone: item.patient_phone,
            patient_address: item.patient_address,
            patient_nik: item.patient_nik,
          };
          this.items = [...this.items, setPatient];
        });
        // console.log(this.items);
      } catch (err) {
        console.log(err);
      }
    },
    async setPatient(data) {
      // console.log(data.item);
      this.form = {
        patient_name: data.item.patient_name,
        patient_sex: data.item.patient_sex,
        patient_religion: data.item.patient_religion,
        patient_phone: data.item.patient_phone,
        patient_address: data.item.patient_address,
        patient_nik: data.item.patient_nik,
      };
      // this.form.patient_name = data.item.patient_name;
      // this.form.patient_sex = data.item.patient_sex;
      // this.form.patient_religion = data.item.patient_religion;
      // this.form.patient_phone = data.item.patient_phone;
      // this.form.patient_address = data.item.patient_address;
      // this.form.patient_nik = data.item.patient_nik;
      this.patient_id = data.item.patient_id;
    },
    async updatePatient() {
      try {
        const payload = {
          id: this.patient_id,
          form: this.form,
        };
        await axios.put(
          `http://localhost:3001/users/${payload.id}`,
          {
            patient_name: this.form.patient_name,
            patient_sex: this.form.patient_sex,
            patient_religion: this.form.patient_religion,
            patient_phone: this.form.patient_phone,
            patient_address: this.form.patient_address,
            patient_nik: this.form.patient_nik,
          },
        ).then((result) => {
          console.log(result);
          this.$bvModal.hide('modal-edit');
          this.getPatients();
        });
        // this.form.patient_name = '';
        // this.form.patient_sex = '';
        // this.form.patient_religion = '';
        // this.form.patient_phone = '';
        // this.form.patient_address = '';
        // this.form.patient_nik = '';
        // this.$router.push('/');
      } catch (err) {
        console.log(err);
      }
    },
    async postPatient() {
      try {
        await axios.post('http://localhost:3001/users/', {
          patient_name: this.form.patient_name,
          patient_sex: this.form.patient_sex,
          patient_religion: this.form.patient_religion,
          patient_phone: this.form.patient_phone,
          patient_address: this.form.patient_address,
          patient_nik: this.form.patient_nik,
        });
        this.form.patient_name = '';
        this.form.patient_sex = '';
        this.form.patient_religion = '';
        this.form.patient_phone = '';
        this.form.patient_address = '';
        this.form.patient_nik = '';
        this.$router.push('/');
      } catch (err) {
        console.log(err);
      }
    },
    deletePatient(data) {
      this.patient_id = data.item.patient_id;
      this.$bvModal.msgBoxConfirm(`Please confirm that you want to delete ${data.item.patient_name}'s data?`, {
        title: 'Delete Patient',
        size: 'sm',
        buttonSize: 'sm',
        okVariant: 'danger',
        okTitle: 'YES',
        cancelTitle: 'NO',
        footerClass: 'p-2',
        hideHeaderClose: false,
        centered: true,
      })
        .then((value) => {
          const payload = {
            id: this.patient_id,
          };
          if (value === true) {
            axios.delete(`http://localhost:3001/users/${payload.id}`)
              .then((result) => {
                console.log(result);
                this.getPatients();
              });
            // this.deletePatients().then((result) => {
            //   console.log(result);
            //   this.getPatients();
            // });
          } else {
            this.getPatients();
          }
        }).catch((err) => {
          console.log(err);
        });
    },
    async deletePatients() {
      try {
        const payload = {
          id: this.patient_id,
          form: this.form,
        };
        await axios.delete(`http://localhost:3001/users/${payload.id}`);
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form-select {
  padding: 0.375rem 0.75rem;
  border: 1px solid #ced4da;
  background-color: #fff;
  font-size: 1rem;
  line-height: 1.5;
  display: block;
  border-radius: 0.25rem;
  width: 100%;
}
</style>
