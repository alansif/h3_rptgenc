<template>
	<v-stepper non-linear>
		<v-stepper-header>
			<v-stepper-step :complete="myFiles1.length>0" step="1" editable>病历</v-stepper-step><v-divider/>
			<v-stepper-step :complete="myFiles2.length>0" step="2" editable>内镜报告</v-stepper-step><v-divider/>
			<v-stepper-step :complete="myFiles3.length>0" step="3" editable>病理报告</v-stepper-step><v-divider/>
			<v-stepper-step :complete="myFiles4.length>0" step="4" editable>化验报告</v-stepper-step><v-divider/>
			<v-stepper-step :complete="myFiles5.length>0" step="5" editable>心电图</v-stepper-step><v-divider/>
			<v-stepper-step :complete="true" step="6" editable>生成</v-stepper-step>
		</v-stepper-header>
		<v-stepper-items>
			<v-stepper-content step="1">
				<FilePond
					label-idle="拖放病历记录到此处..."
					:allow-multiple="true"
					accepted-file-types="image/jpeg, image/png"
					:files="myFiles1"
				/>
			</v-stepper-content>
		</v-stepper-items>
		<v-stepper-items>
			<v-stepper-content step="2">
				<FilePond
					label-idle="拖放内镜报告到此处..."
					:allow-multiple="true"
					accepted-file-types="image/jpeg, image/png"
					:files="myFiles2"
				/>
			</v-stepper-content>
		</v-stepper-items>
		<v-stepper-items>
			<v-stepper-content step="3">
				<FilePond
					label-idle="拖放病理报告到此处..."
					:allow-multiple="true"
					accepted-file-types="image/jpeg, image/png"
					:files="myFiles3"
				/>
			</v-stepper-content>
		</v-stepper-items>
		<v-stepper-items>
			<v-stepper-content step="4">
				<FilePond
					label-idle="拖放化验报告到此处..."
					:allow-multiple="true"
					accepted-file-types="image/jpeg, image/png"
					:files="myFiles4"
				/>
			</v-stepper-content>
		</v-stepper-items>
		<v-stepper-items>
			<v-stepper-content step="5">
				<FilePond
					label-idle="拖放心电图文件到此处..."
					:allow-multiple="true"
					accepted-file-types="image/jpeg, image/png"
					:files="myFiles5"
				/>
			</v-stepper-content>
		</v-stepper-items>
		<v-stepper-items>
			<v-stepper-content step="6">
			</v-stepper-content>
		</v-stepper-items>
	</v-stepper>
</template>

<script>
import vueFilePond from 'vue-filepond';
import FilePondPluginFileValidateType from 'filepond-plugin-file-validate-type/dist/filepond-plugin-file-validate-type.esm.js';
import FilePondPluginImagePreview from 'filepond-plugin-image-preview/dist/filepond-plugin-image-preview.esm.js';
import 'filepond/dist/filepond.min.css';
import 'filepond-plugin-image-preview/dist/filepond-plugin-image-preview.min.css';
const FilePond = vueFilePond( FilePondPluginFileValidateType, FilePondPluginImagePreview );

export default {
	data() {
		return {
			myFiles1: [],
			myFiles2: [],
			myFiles3: [],
			myFiles4: [],
			myFiles5: [],
		}
	},
	components: {
        FilePond
    }
}
</script>
