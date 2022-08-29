<template>
    <div v-if="display" class="card">
        <div v-if="user.brandId == 1" class="card">
            <QCallFormTech v-model="user" ref="qCallFormTech" />
        </div>
        <div v-if="user.brandId == 2" class="card">
            <QCallFormFinance v-model="user" ref="qCallFormFinance" />
        </div>
        <QCallFormGeneralCandidateInfo v-model="user" ref="qCallFormGeneralCandidateInfo" />
        <QCallFormOwnership v-model="user" ref="qCallFormOwnership" />
    </div>
</template>
<script>
    import { ref, nextTick } from 'vue';
    import { useI18n } from 'vue-i18n';
    import QCallFormOwnership from './QCallFormOwnership.vue';
    import QCallFormGeneralCandidateInfo from './QCallFormGeneralCandidateInfo.vue';
    import QCallFormFinance from "@/admin/candidates/QCallFormFinance";
    import QCallFormTech from "@/admin/candidates/QCallFormTech";
    import useVuelidate from "@vuelidate/core";

    export default {
        components: {
            QCallFormFinance,
            QCallFormTech,
            QCallFormOwnership,
            QCallFormGeneralCandidateInfo,
        },
        props: ["modelValue"],
        setup(props) {
            const { t } = useI18n();
            const qCallFormOwnership = ref();
            const qCallFormGeneralCandidateInfo = ref();
            const qCallFormFinance = ref();
            const qCallFormTech = ref();
            const user = ref(props.modelValue);
            const display = ref(false);

            const show = async () => {
                display.value = true;
                nextTick(() => document.getElementsByClassName('p-dialog-content')[0].scrollTo(0, 0));
            }
            const close = () => { display.value = false; }

            const rules = {

            };

            const v$ = useVuelidate(rules, user);

            const validate = () => {
                v$.value.$touch();
                if (user.value.brandId == 1) {
                    qCallFormTech.value.validate();
                }
                if (user.value.brandId == 2) {
                    qCallFormFinance.value.validate();
                }
                qCallFormOwnership.value.validate();
                qCallFormGeneralCandidateInfo.value.validate();
                return v$.value.$error;
            }

            return {
                display,
                user,
                validate,
                t,
                v$,
                show,
                close,
                qCallFormFinance,
                qCallFormTech,
                qCallFormOwnership,
                qCallFormGeneralCandidateInfo
            };
        }
    }
</script>