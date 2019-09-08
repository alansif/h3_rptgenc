<template>
	<v-stepper non-linear>
		<v-stepper-header>
			<v-stepper-step :complete="true" step="0" editable>基本信息</v-stepper-step><v-divider/>
			<v-stepper-step :complete="hasfile1" step="1" :editable="pedscp.length>0">病历</v-stepper-step><v-divider/>
			<v-stepper-step :complete="hasfile2" step="2" :editable="pedscp.length>0">内镜报告</v-stepper-step><v-divider/>
			<v-stepper-step :complete="hasfile3" step="3" :editable="pedscp.length>0">病理报告</v-stepper-step><v-divider/>
			<v-stepper-step :complete="hasfile4" step="4" :editable="pedscp.length>0">化验报告</v-stepper-step><v-divider/>
			<v-stepper-step :complete="hasfile5" step="5" :editable="pedscp.length>0">心电图</v-stepper-step><v-divider/>
			<v-stepper-step :complete="pedscp.length>0" step="6" :editable="pedscp.length>0">生成</v-stepper-step>
		</v-stepper-header>
		<v-stepper-items>
			<v-stepper-content step="0">
				<v-card max-width="400" class="mx-auto pa-4" outlined>
					<v-card-text>
						<v-form>
							<v-text-field label="病案号" outlined append-icon="search" v-model="pid"
								@click:append="search" @keyup.enter.native="search" @input="clearall"/>
							<v-text-field label="姓名" readonly v-model="pname"></v-text-field>
							<v-text-field label="性别" readonly v-model="psex"></v-text-field>
							<v-text-field label="检查日期" readonly v-model="pexamdate"></v-text-field>
							<v-select label="检查项目" chips multiple readonly append-icon class="tag-input"
								:items="['无痛胃镜','无痛肠镜']" v-model="pedscp">
							</v-select>
						</v-form>
					</v-card-text>
				</v-card>
			</v-stepper-content>
		</v-stepper-items>
		<v-stepper-items>
			<v-stepper-content step="1">
				<FilePond
					ref="fp1"
					label-idle="拖放病历记录到此处..."
					:allow-multiple="true"
					accepted-file-types="image/jpeg, image/png"
					:files="myFiles1"
					:imagePreviewMaxHeight="512"
					name="files"
					server="/api/upload"
					@updatefiles="updatefiles"
				/>
			</v-stepper-content>
		</v-stepper-items>
		<v-stepper-items>
			<v-stepper-content step="2">
				<FilePond
					ref="fp2"
					label-idle="拖放内镜报告到此处..."
					:allow-multiple="true"
					accepted-file-types="image/jpeg, image/png"
					v-bind:files="myFiles2"
					:imagePreviewMaxHeight="512"
					name="files"
					server="/api/upload"
					@updatefiles="updatefiles"
				/>
			</v-stepper-content>
		</v-stepper-items>
		<v-stepper-items>
			<v-stepper-content step="3">
				<FilePond
					ref="fp3"
					label-idle="拖放病理报告到此处..."
					:allow-multiple="true"
					accepted-file-types="image/jpeg, image/png"
					:files="myFiles3"
					:imagePreviewMaxHeight="512"
					name="files"
					server="/api/upload"
					@updatefiles="updatefiles"
				/>
			</v-stepper-content>
		</v-stepper-items>
		<v-stepper-items>
			<v-stepper-content step="4">
				<FilePond
					ref="fp4"
					label-idle="拖放化验报告到此处..."
					:allow-multiple="true"
					accepted-file-types="image/jpeg, image/png"
					:files="myFiles4"
					:imagePreviewMaxHeight="512"
					name="files"
					server="/api/upload"
					@updatefiles="updatefiles"
				/>
			</v-stepper-content>
		</v-stepper-items>
		<v-stepper-items>
			<v-stepper-content step="5">
				<FilePond
					ref="fp5"
					label-idle="拖放心电图文件到此处..."
					:allow-multiple="true"
					accepted-file-types="image/jpeg, image/png"
					:files="myFiles5"
					:imagePreviewMaxHeight="512"
					name="files"
					server="/api/upload"
					@updatefiles="updatefiles"
				/>
			</v-stepper-content>
		</v-stepper-items>
		<v-stepper-items>
			<v-stepper-content step="6">
				<v-card max-width="400" class="mx-auto pa-4 text-center" flat>
					<v-btn x-large rounded color="primary" elevation="6" width="200" :loading="loading" @click="gen">生成报告</v-btn>
				</v-card>
			</v-stepper-content>
		</v-stepper-items>
		<v-snackbar v-model="snackbar" color="blue lighten-1" top>无符合条件的记录</v-snackbar>
		<v-dialog v-model="loading" persistent width="400">
			<v-card color="primary" dark>
				<v-card-text class="pt-4">
					正在生成报告...
					<v-progress-linear indeterminate color="white" class="mb-0"/>
				</v-card-text>
			</v-card>
		</v-dialog>		
	</v-stepper>
</template>

<script>
import vueFilePond from 'vue-filepond';
import FilePondPluginFileValidateType from 'filepond-plugin-file-validate-type/dist/filepond-plugin-file-validate-type.esm.js';
import FilePondPluginImagePreview from 'filepond-plugin-image-preview/dist/filepond-plugin-image-preview.esm.js';
import 'filepond/dist/filepond.min.css';
import 'filepond-plugin-image-preview/dist/filepond-plugin-image-preview.min.css';
const FilePond = vueFilePond( FilePondPluginFileValidateType, FilePondPluginImagePreview );
import fileDownload from 'js-file-download';

export default {
	data() {
		return {
			myFiles1: [],
			myFiles2: [],
			myFiles3: [],
			myFiles4: [],
			myFiles5: [],
			hasfile1: false,
			hasfile2: false,
			hasfile3: false,
			hasfile4: false,
			hasfile5: false,
			pid: '',
			pname: '',
			psex: '',
			pexamdate: '',
			pedscp: [],
			snackbar: false,
			loading: false
		}
	},
	methods: {
		search() {
			if (this.pid.length === 0) return;
			this.$axios.get('/api/query',{params:{
					pid: this.pid
				}})
				.then(response => {
					const items = response.data;
					if (items.length === 0) {
						this.snackbar = true;
					} else {
						this.showItems(items);
					}
				})
				.catch(error => {
					console.dir(error);
				});
		},
		showItems(items) {
			this.pname = items[0].XM1;
			this.psex = items[0].XB1;
			this.pexamdate = items[0].ScheduleDate;
			this.pedscp = items.map(v => v.SQMD);
		},
		clearall() {
			this.pname = '';
			this.psex = '';
			this.pexamdate = '';
			this.pedscp = [];
		},
		gen() {
			let rptparams = {
				id: this.pid,
				name: this.pname,
				sex: this.psex,
				examdate: this.pexamdate,
				edscp: this.pedscp,
				files: []
			};
			for(let i = 0; i <5; ++i) {
				const f = this.$refs['fp'+(i+1)].getFiles();
				rptparams.files.push(f.map(v => ({id:v.serverId, ext:v.fileExtension}) ));
			}
			this.loading = true;
			this.$axios.post('/api/make', rptparams, {responseType:'blob'})
				.then(response => {
					fileDownload(response.data, this.pid + this.pname +".pdf");
				})
				.catch(error => {
					if (error) {
						console.dir(error);
					}
				})
				.finally(() => {
					this.loading = false;
				});
		},
		updatefiles() {
			this.hasfile1 = this.$refs['fp1'] && (this.$refs['fp1'].getFiles().length > 0);
			this.hasfile2 = this.$refs['fp2'] && (this.$refs['fp2'].getFiles().length > 0);
			this.hasfile3 = this.$refs['fp3'] && (this.$refs['fp3'].getFiles().length > 0);
			this.hasfile4 = this.$refs['fp4'] && (this.$refs['fp4'].getFiles().length > 0);
			this.hasfile5 = this.$refs['fp5'] && (this.$refs['fp5'].getFiles().length > 0);
		}
	},
	components: {
        FilePond
    }
}
</script>

<style>
.tag-input span.v-chip {
  background-color: #0000ef!important;
  color: #fff!important;
}
</style>
