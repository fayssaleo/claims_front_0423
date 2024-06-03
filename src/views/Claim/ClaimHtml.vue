<template>
  <span>
    <div
      class="claim-details"
      style="padding: 2px 41px"
      v-for="claim in claims"
      :key="claim.id"
    >
      <div
        class="involved-parties"
        v-for="equipment in claim.equipments"
        :key="equipment.id"
      >
        <span class="logo">
          <img src="@/assets/TangerAlliance_Cropped.png" alt="TA LOGO" />
        </span>
        <h2
          style="
            display: inline-block;
            width: 50%;
            float: right;
            margin-top: 15px;
            text-align: right;
          "
        >
          <strong>CLAIM : </strong>{{ getClaimSerialNumber(claim) || "Empty" }}
        </h2>
        <h2
          style="
            margin: 13px 11px;
            display: inline-block;
            margin-bottom: 35px;
            width: 100%;
            text-align: center;
            margin-top: -25px;
          "
        >
          <strong>SUBJECT : </strong>EQUIPMENT
          {{ equipment.type_of_equipment?.name || "Empty" }}
          {{ equipment.matricule.id_equipment || "Empty" }}
        </h2>
        <div class="claim-info">
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>Claim Date :</strong>
            {{ claim.claim_date || "Empty" }}</span
          >

          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>Status :</strong> {{ claim.status || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>REINVOICED :</strong>
            {{ equipment.reinvoiced || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>Incident Date :</strong>
            {{ claim.incident_date || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>CATEGORY :</strong> {{ claim.category }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>TOTAL ESTIMATION :</strong>
            {{ claim.totalEstimation || "Empty" }}</span
          >
        </div>

        <h2 style="margin-top: 38px; margin-bottom: 38px">
          Involved Parties :
        </h2>
        <h3
          style="
            margin-bottom: 5px;
            background-color: rgb(11 54 89);
            color: white;
            width: 100%;
          "
        >
          EQUIPMENT :
        </h3>
        <div class="claim-info">
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>EQUIPMENT ID :</strong>
            {{ equipment.matricule.id_equipment || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>STATUS :</strong>
            {{
              equipment.nature_of_damage.id == 0
                ? "Not Damaged"
                : "Damaged" || "Empty"
            }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>MATRICULE :</strong>
            {{ equipment.matricule?.matricule || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>DECLARED :</strong>
            {{
              equipment.date_of_declaration == null
                ? " Undeclared"
                : "Declared" || "Empty"
            }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>TYPE :</strong>
            {{ equipment.type_of_equipment?.name || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>REINVOICED :</strong>
            {{ equipment.reinvoiced || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>BRAND :</strong>
            {{ equipment.brand?.name || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>Total estimation :</strong>
            {{ getTotalToTo(equipment.estimate) || "Empty" }}</span
          >
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            DAMAGE INFORMATION :
          </h4>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
            }"
            style="width: 90% !important"
            ><strong>CONCERNED DEPARTMENTS :</strong>
            {{
              getThisEquipmentDepartments(
                equipment.concerned_internal_department
              ) || "Empty"
            }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>NATURE OF DAMAGE :</strong>
            {{ equipment.nature_of_damage.name || "Empty" }}</span
          >

          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>CAUSE OF DAMAGE :</strong>
            {{ equipment.cause_damage || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>CAUSED BY :</strong>
            {{ equipment.damage_caused_by || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
            }"
            v-if="
              equipment.damage_caused_by == 'TangerAlliance' &&
              equipment.TAT_name_persons != null
            "
            ><strong>Name Of TAT people : :</strong>
            <span
              style="display: block"
              v-if="equipment.TAT_name_persons!=null && equipment.TAT_name_persons!=''"
              v-for="person in equipment.TAT_name_persons?.split('|')"
              :key="person"
              cols="12"
              >{{ person }}</span
            >
          </span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
            }"
            v-if="equipment.damage_caused_by == 'Outsourcer'"
          >
            <strong style="display: block">COMPANY :</strong
            >{{ equipment.companie.name }}
            <strong style="display: block">NAME OF OUTSOURCER PEOPLE :</strong>
            <span
              style="display: block"
              v-if="equipment.outsourcer_persons!=null && equipment.outsourcer_persons!=''"
              v-for="person in equipment.outsourcer_persons?.split('|')"
              :key="person"
              cols="12"
              >{{ person }}</span
            >
          </span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
            }"
            v-if="equipment.damage_caused_by == 'Thirdparty'"
          >
            <strong>THIRDPARTY COMPANY :</strong
            >{{ equipment.thirdparty_company_name }}
            <strong style="display: block">THIRDPARTY ACTIVITY :</strong
            >{{ equipment.thirdparty_Activity_comments }}
            <strong style="display: block">NAME OF THIRDPARTY PEOPLE :</strong>
            <span
              style="display: block"
              v-if="equipment.thirdparty_persons!=null && equipment.thirdparty_persons!=''"
              v-for="person in equipment.thirdparty_persons?.split('|')"
              :key="person"
              cols="12"
              >{{ person }}</span
            >
          </span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
            }"
            style="width: 100% !important"
            ><strong>DAMAGE DESCRIPTION :</strong>
            {{ equipment.damage_description || "Empty" }}</span
          >
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            ESTIMATIONS INFORMATION :
            <span
              style="
                font-style: italic;
                font-size: 13px;
                font-weight: 100;
                color: #ffffff5e;
              "
              v-if="equipment.estimate.length <= 0"
              >Empty</span
            >
          </h4>
          <table style="width: 100%" v-if="equipment.estimate.length > 0">
            <tr>
              <th style="width: 25%">Name</th>
              <th style="width: 25%">value</th>
              <th style="width: 10%">Currency</th>
              <th style="width: 15%">Exchange rate</th>
              <th style="width: 25%">Valuation value</th>
            </tr>
          </table>
          <table
            style="width: 100%"
            v-for="oneEstimate in equipment.estimate"
            :key="oneEstimate.id"
            v-if="equipment.estimate.length > 0"
          >
            <tr
              v-for="Oneother_valuation in oneEstimate.other_valuation"
              :key="Oneother_valuation.id"
            >
              <td style="width: 25%; border: 1px solid black">
                <strong>
                  {{ Oneother_valuation.name }}
                </strong>
              </td>
              <td style="width: 25%; border: 1px solid black">
                {{ Math.floor(Oneother_valuation.value * 100) / 100 }}
              </td>
              <td style="width: 10%; border: 1px solid black">
                {{ Oneother_valuation.currency }}
              </td>
              <td style="width: 15%; border: 1px solid black">
                {{ Oneother_valuation.taux_change }}
              </td>
              <td style="width: 25%; border: 1px solid black">
                {{ Math.floor(Oneother_valuation.value_valuation * 100) / 100 }}
              </td>
            </tr>
            <tr style="width: 100%">
              <td style="width: 25%"></td>
              <td style="width: 25%"></td>
              <td style="width: 10%"></td>
              <td style="width: 15%">Total estimation :</td>
              <td style="width: 25%">
                {{ Math.floor(getTotal(oneEstimate) * 100) / 100 }} (€)
              </td>
            </tr>
          </table>
          <table style="width: 100%" v-if="equipment.estimate.length > 0">
            <tr>
              <td style="width: 25%"></td>
              <td style="width: 25%"></td>
              <td style="width: 15%"></td>
              <td style="width: 10%"><strong>Total : </strong></td>
              <td style="width: 25%">
                <strong>{{ getTotalToTo(equipment.estimate) }} </strong>
              </td>
            </tr>
          </table>
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            REINVOICED
          </h4>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>AMOUNT :</strong>
            {{ Math.floor(equipment.amount * 100) / 100 || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>CURRENCY :</strong>
            {{ equipment.currency || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>COMMENT :</strong>
            {{ equipment.comment_third_party || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>REINVOICED :</strong>
            {{ equipment.reinvoiced || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>REIMBURSMENT DATE :</strong>
            {{ equipment.date_of_reimbursement || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
            }"
            v-if="equipment.reinvoiced == 'REINVOICED'"
            ><strong>REIMBURSMENT DATE :</strong>
            {{ equipment.Invoice_number || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
            }"
            v-if="equipment.reinvoiced == 'REINVOICED'"
            ><strong>REIMBURSMENT AMOUNT :</strong>
            {{ equipment.reimbursed_amount || "Empty" }}</span
          >
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            INSURANCE DECLARATION & FOLLOW UP
          </h4>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>DECLARATION DATE :</strong>
            {{ equipment.date_of_declaration || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>DATE OF FEEDBACK :</strong>
            {{ equipment.date_of_feedback || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
              marginTop: 20 - equipment.estimate.length + 'px',
            }"
            ><strong>COMMENT :</strong>
            {{ equipment.Indemnification_of_insurer || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
            }"
            style="width: 50%"
            ><strong>INDEMNIFICATION OF THE INSURER :</strong>
            {{ equipment.Indemnification_of_insurer || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
            }"
            style="width: 45%"
            ><strong>DEDUCTIBLE IN CHARGE OF TAT :</strong>
            {{ equipment.deductible_charge_TAT || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
            }"
            style="width: 50%"
            ><strong>CURRENCY :</strong>
            {{ equipment.currency_indemnisation || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - equipment.estimate.length + 'px',
            }"
            style="width: 45%"
            ><strong>INDEMNIFICATION DATE :</strong>
            {{ equipment.Indemnification_date || "Empty" }}</span
          >
        </div>
        <div class="html2pdf__page-break"></div>
      </div>

      <div
        class="involved-parties"
        v-for="automobile in claim.automobiles"
        :key="automobile.id"
      >
        <span class="logo">
          <img src="@/assets/TangerAlliance_Cropped.png" alt="TA LOGO" />
        </span>
        <h2
          style="
            display: inline-block;
            width: 50%;
            float: right;
            margin-top: 15px;
            text-align: right;
          "
        >
          <strong>CLAIM : </strong>{{ getClaimSerialNumber(claim) || "Empty" }}
        </h2>
        <h2
          style="
            margin: 13px 11px;
            display: inline-block;
            margin-bottom: 8px;
            width: 100%;
            text-align: center;
            margin-top: -25px;
          "
        >
          <strong>SUBJECT : </strong>automobile
          {{ automobile.type_of_equipment?.name || "Empty" }}
          {{ automobile.matricule.id_equipment || "Empty" }}
        </h2>
        <div class="claim-info">
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>Claim Date :</strong>
            {{ claim.claim_date || "Empty" }}</span
          >

          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>Status :</strong> {{ claim.status || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>REINVOICED :</strong>
            {{ automobile.reinvoiced || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>Incident Date :</strong>
            {{ claim.incident_date || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>CATEGORY :</strong> {{ claim.category }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>TOTAL ESTIMATION :</strong>
            {{ claim.totalEstimation || "Empty" }}</span
          >
        </div>

        <h2
          style="
            margin: 6px 11px -2px 20px;
            margin-top: 38px;
            margin-bottom: 38px;
          "
        >
          Involved Parties :
        </h2>
        <h3
          style="
            margin-bottom: 5px;
            background-color: rgb(11 54 89);
            color: white;
            width: 100%;
          "
        >
          AUTOMOBILE :
        </h3>
        <div class="claim-info">
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>AUTOMOBILE ID :</strong>
            {{ automobile.matricule.id_equipment || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>STATUS :</strong>
            {{
              automobile.nature_of_damage.id == 0
                ? "Not Damaged"
                : "Damaged" || "Empty"
            }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>MATRICULE :</strong>
            {{ automobile.matricule?.matricule || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>DECLARED :</strong>
            {{
              automobile.date_of_declaration == null
                ? " Undeclared"
                : "Declared" || "Empty"
            }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>TYPE :</strong>
            {{ automobile.type_of_equipment?.name || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>REINVOICED :</strong>
            {{ automobile.reinvoiced || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>BRAND :</strong>
            {{ automobile.brand?.name || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>Total estimation :</strong>
            {{ getTotalToTo(automobile.estimate) || "Empty" }}</span
          >
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            DAMAGE INFORMATION :
          </h4>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            style="width: 90% !important"
            ><strong>CONCERNED DEPARTMENTS :</strong>
            {{
              getThisEquipmentDepartments(
                automobile.concerned_internal_department
              ) || "Empty"
            }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>NATURE OF DAMAGE :</strong>
            {{ automobile.nature_of_damage.name || "Empty" }}</span
          >

          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>CAUSE OF DAMAGE :</strong>
            {{ automobile.cause_damage || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>CAUSED BY :</strong>
            {{ automobile.damage_caused_by || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            v-if="
              automobile.damage_caused_by == 'TangerAlliance' &&
              automobile.TAT_name_persons != null
            "
            ><strong>Name Of TAT people : :</strong>
            <span
              style="display: block"
              v-if="automobile.TAT_name_persons!=null && automobile.TAT_name_persons!=''"
              v-for="person in automobile.TAT_name_persons?.split('|')"
              :key="person"
              cols="12"
              >{{ person }}</span
            >
          </span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            v-if="automobile.damage_caused_by == 'Outsourcer'"
          >
            <strong style="display: block">COMPANY :</strong
            >{{ automobile.companie.name }}
            <strong style="display: block">NAME OF OUTSOURCER PEOPLE :</strong>
            <span
              style="display: block"
              v-if="automobile.outsourcer_persons!=null && automobile.outsourcer_persons!=''"
              v-for="person in automobile.outsourcer_persons?.split('|')"
              :key="person"
              cols="12"
              >{{ person }}</span
            >
          </span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            v-if="automobile.damage_caused_by == 'Thirdparty'"
          >
            <strong>THIRDPARTY COMPANY :</strong
            >{{ automobile.thirdparty_company_name }}
            <strong style="display: block">THIRDPARTY ACTIVITY :</strong
            >{{ automobile.thirdparty_Activity_comments }}
            <strong style="display: block">NAME OF THIRDPARTY PEOPLE :</strong>
            <span
              style="display: block"
              v-if="automobile.thirdparty_persons!=null && automobile.thirdparty_persons!=''"
              v-for="person in automobile.thirdparty_persons?.split('|')"
              :key="person"
              cols="12"
              >{{ person }}</span
            >
          </span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            style="width: 100% !important"
            ><strong>DAMAGE DESCRIPTION :</strong>
            {{ automobile.damage_description || "Empty" }}</span
          >
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            ESTIMATIONS INFORMATION :
            <span v-if="automobile.estimate.length <= 0">Empty</span>
          </h4>
          <table style="width: 100%" v-if="automobile.estimate.length > 0">
            <tr>
              <th style="width: 25%">Name</th>
              <th style="width: 25%">value</th>
              <th style="width: 10%">Currency</th>
              <th style="width: 15%">Exchange rate</th>
              <th style="width: 25%">Valuation value</th>
            </tr>
          </table>
          <table
            style="width: 100%"
            v-for="oneEstimate in automobile.estimate"
            :key="oneEstimate.id"
            v-if="automobile.estimate.length > 0"
          >
            <tr
              v-for="Oneother_valuation in oneEstimate.other_valuation"
              :key="Oneother_valuation.id"
            >
              <td style="width: 25%; border: 1px solid black">
                <strong>
                  {{ Oneother_valuation.name }}
                </strong>
              </td>
              <td style="width: 25%; border: 1px solid black">
                {{ Math.floor(Oneother_valuation.value * 100) / 100 }}
              </td>
              <td style="width: 10%; border: 1px solid black">
                {{ Oneother_valuation.currency }}
              </td>
              <td style="width: 15%; border: 1px solid black">
                {{ Oneother_valuation.taux_change }}
              </td>
              <td style="width: 25%; border: 1px solid black">
                {{ Math.floor(Oneother_valuation.value_valuation * 100) / 100 }}
              </td>
            </tr>
            <tr style="width: 100%" v-if="automobile.estimate.length > 0">
              <td style="width: 25%"></td>
              <td style="width: 25%"></td>
              <td style="width: 10%"></td>
              <td style="width: 15%">Total estimation :</td>
              <td style="width: 25%">
                {{ Math.floor(getTotal(oneEstimate) * 100) / 100 }} (€)
              </td>
            </tr>
          </table>
          <table style="width: 100%" v-if="automobile.estimate.length > 0">
            <tr>
              <td style="width: 25%"></td>
              <td style="width: 25%"></td>
              <td style="width: 15%"></td>
              <td style="width: 10%"><strong>Total : </strong></td>
              <td style="width: 25%">
                <strong>{{ getTotalToTo(automobile.estimate) }} </strong>
              </td>
            </tr>
          </table>
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            REINVOICED
          </h4>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>AMOUNT :</strong>
            {{ Math.floor(automobile.amount * 100) / 100 || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>CURRENCY :</strong>
            {{ automobile.currency || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>COMMENT :</strong>
            {{ automobile.comment_third_party || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>REINVOICED :</strong>
            {{ automobile.reinvoiced || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>REIMBURSMENT DATE :</strong>
            {{ automobile.date_of_reimbursement || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            v-if="automobile.reinvoiced == 'REINVOICED'"
            ><strong>REIMBURSMENT DATE :</strong>
            {{ automobile.Invoice_number || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            v-if="automobile.reinvoiced == 'REINVOICED'"
            ><strong>REIMBURSMENT AMOUNT :</strong>
            {{ automobile.reimbursed_amount || "Empty" }}</span
          >
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            INSURANCE DECLARATION & FOLLOW UP
          </h4>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>DECLARATION DATE :</strong>
            {{ automobile.date_of_declaration || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>DATE OF FEEDBACK :</strong>
            {{ automobile.date_of_feedback || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            ><strong>COMMENT :</strong>
            {{ automobile.Indemnification_of_insurer || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            style="width: 50%"
            ><strong>INDEMNIFICATION OF THE INSURER :</strong>
            {{ automobile.Indemnification_of_insurer || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            style="width: 45%"
            ><strong>DEDUCTIBLE IN CHARGE OF TAT :</strong>
            {{ automobile.deductible_charge_TAT || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            style="width: 45%"
            ><strong>CURRENCY :</strong>
            {{ automobile.currency_indemnisation || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - automobile.estimate.length + 'px',
              marginTop: 20 - automobile.estimate.length + 'px',
            }"
            style="width: 45%"
            ><strong>INDEMNIFICATION DATE :</strong>
            {{ automobile.Indemnification_date || "Empty" }}</span
          >
        </div>
        <div class="html2pdf__page-break"></div>
      </div>

      <div
        class="involved-parties"
        v-for="container in claim.containers"
        :key="container.id"
      >
        <span class="logo">
          <img src="@/assets/TangerAlliance_Cropped.png" alt="TA LOGO" />
        </span>
        <h2
          style="
            display: inline-block;
            width: 50%;
            float: right;
            margin-top: 15px;
            text-align: right;
          "
        >
          <strong>CLAIM : </strong>{{ getClaimSerialNumber(claim) || "Empty" }}
        </h2>
        <h2
          style="
            margin: 13px 11px;
            display: inline-block;
            margin-bottom: 35px;
            width: 100%;
            text-align: center;
            margin-top: -25px;
          "
        >
          <strong>SUBJECT : </strong>
          {{ container.shipping_line.name || "Empty" }} CONTAINER -
          {{ container.containerID || "Empty" }}
        </h2>
        <div class="claim-info">
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>Claim Date :</strong>
            {{ claim.claim_date || "Empty" }}</span
          >

          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>Status :</strong> {{ claim.status || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>REINVOICED :</strong>
            {{ container.reinvoiced || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>Incident Date :</strong>
            {{ claim.incident_date || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>CATEGORY :</strong> {{ claim.category }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>Total Estimation :</strong>
            {{ claim.totalEstimation || "Empty" }}</span
          >
        </div>

        <h2
          style="
            margin: 6px 11px -2px 20px;
            margin-top: 38px;
            margin-bottom: 38px;
          "
        >
          Involved Parties :
        </h2>
        <h3
          style="
            margin-bottom: 5px;
            background-color: rgb(11 54 89);
            color: white;
            width: 100%;
          "
        >
          CONTAINER :
        </h3>
        <div class="claim-info">
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>CONTAINER ID :</strong>
            {{ container.containerID || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>STATUS :</strong>
            {{
              container.nature_of_damage.id == 0
                ? "Not Damaged"
                : "Damaged" || "Empty"
            }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>MARCHANDISE :</strong>
            {{ container.marchandise == "NO" ? "NO" : "YES" || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>SHIPPINE LINE :</strong>
            {{ container.shipping_line.name || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>DECLARED :</strong>
            {{
              container.date_of_declaration == null
                ? " Undeclared"
                : "Declared" || "Empty"
            }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>REINVOICED :</strong>
            {{ container.reinvoiced || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>TYPE :</strong>
            {{ container.containerType || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
          ></span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>TOTAL ESTIMATION :</strong>
            {{ getTotalToTo(container.estimate) || "Empty" }}</span
          >
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            DAMAGE INFORMATION :
          </h4>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            style="width: 90% !important"
            ><strong>CONCERNED DEPARTMENTS :</strong>
            {{
              getThisEquipmentDepartments(
                container.concerned_internal_department
              ) || "Empty"
            }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>NATURE OF DAMAGE :</strong>
            {{ container.nature_of_damage.name || "Empty" }}</span
          >

          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>CAUSE OF DAMAGE :</strong>
            {{ container.cause_damage || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>CAUSED BY :</strong>
            {{ container.damage_caused_by || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            v-if="
              container.damage_caused_by == 'TangerAlliance' &&
              container.TAT_name_persons != null
            "
            ><strong>Name Of TAT people : :</strong>
            <span
              style="display: block"
              v-if="container.TAT_name_persons!=null && container.TAT_name_persons!=''"
              v-for="person in container.TAT_name_persons?.split('|')"
              :key="person"
              cols="12"
              >{{ person }}</span
            >
          </span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            v-if="container.damage_caused_by == 'Outsourcer'"
          >
            <strong style="display: block">COMPANY :</strong
            >{{ container.companie.name }}
            <strong style="display: block">NAME OF OUTSOURCER PEOPLE :</strong>
            <span
              style="display: block"
              v-if="container.outsourcer_persons!=null && container.outsourcer_persons!=''"
              v-for="person in container.outsourcer_persons?.split('|')"
              :key="person"
              cols="12"
              >{{ person }}</span
            >
          </span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            v-if="container.damage_caused_by == 'Thirdparty'"
          >
            <strong>THIRDPARTY COMPANY :</strong
            >{{ container.thirdparty_company_name }}
            <strong style="display: block">THIRDPARTY ACTIVITY :</strong
            >{{ container.thirdparty_Activity_comments }}
            <strong style="display: block">NAME OF THIRDPARTY PEOPLE :</strong>
            <span
              style="display: block"
              v-if="container.thirdparty_persons!=null && container.thirdparty_persons!=''"
              v-for="person in container.thirdparty_persons?.split('|')"
              :key="person"
              cols="12"
              >{{ person }}</span
            >
          </span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            style="width: 100% !important"
            ><strong>DAMAGE DESCRIPTION :</strong>
            {{ container.damage_description || "Empty" }}</span
          >
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            ESTIMATIONS INFORMATION :
            <span
              style="
                font-style: italic;
                font-size: 13px;
                font-weight: 100;
                color: #ffffff5e;
              "
              v-if="container.estimate.length <= 0"
              >Empty</span
            >
          </h4>
          <table style="width: 100%" v-if="container.estimate.length > 0">
            <tr>
              <th style="width: 25%">Name</th>
              <th style="width: 25%">value</th>
              <th style="width: 10%">Currency</th>
              <th style="width: 15%">Exchange rate</th>
              <th style="width: 25%">Valuation value</th>
            </tr>
          </table>
          <table
            style="width: 100%"
            v-for="oneEstimate in container.estimate"
            :key="oneEstimate.id"
            v-if="container.estimate.length > 0"
          >
            <tr
              v-for="Oneother_valuation in oneEstimate.other_valuation"
              :key="Oneother_valuation.id"
            >
              <td style="width: 25%; border: 1px solid black">
                <strong>
                  {{ Oneother_valuation.name }}
                </strong>
              </td>
              <td style="width: 25%; border: 1px solid black">
                {{ Math.floor(Oneother_valuation.value * 100) / 100 }}
              </td>
              <td style="width: 10%; border: 1px solid black">
                {{ Oneother_valuation.currency }}
              </td>
              <td style="width: 15%; border: 1px solid black">
                {{ Oneother_valuation.taux_change }}
              </td>
              <td style="width: 25%; border: 1px solid black">
                {{ Math.floor(Oneother_valuation.value_valuation * 100) / 100 }}
              </td>
            </tr>
            <tr style="width: 100%">
              <td style="width: 25%"></td>
              <td style="width: 25%"></td>
              <td style="width: 10%"></td>
              <td style="width: 15%">Total estimation :</td>
              <td style="width: 25%">
                {{ Math.floor(getTotal(oneEstimate) * 100) / 100 }} (€)
              </td>
            </tr>
          </table>
          <table style="width: 100%" v-if="container.estimate.length > 0">
            <tr>
              <td style="width: 25%"></td>
              <td style="width: 25%"></td>
              <td style="width: 15%"></td>
              <td style="width: 10%"><strong>Total : </strong></td>
              <td style="width: 25%">
                <strong>{{ getTotalToTo(container.estimate) }} </strong>
              </td>
            </tr>
          </table>
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            REINVOICED
          </h4>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>AMOUNT :</strong>
            {{ Math.floor(container.amount * 100) / 100 || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>CURRENCY :</strong>
            {{ container.currency || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>COMMENT :</strong>
            {{ container.comment_third_party || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>REINVOICED :</strong>
            {{ container.reinvoiced || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>REIMBURSMENT DATE :</strong>
            {{ container.date_of_reimbursement || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            v-if="container.reinvoiced == 'REINVOICED'"
            ><strong>REIMBURSMENT DATE :</strong>
            {{ container.Invoice_number || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            v-if="container.reinvoiced == 'REINVOICED'"
            ><strong>REIMBURSMENT AMOUNT :</strong>
            {{ container.reimbursed_amount || "Empty" }}</span
          >
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            INSURANCE DECLARATION & FOLLOW UP
          </h4>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>DECLARATION DATE :</strong>
            {{ container.date_of_declaration || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>DATE OF FEEDBACK :</strong>
            {{ container.date_of_feedback || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            ><strong>COMMENT :</strong>
            {{ container.Indemnification_of_insurer || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            style="width: 50%"
            ><strong>INDEMNIFICATION OF THE INSURER :</strong>
            {{ container.Indemnification_of_insurer || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            style="width: 45%"
            ><strong>DEDUCTIBLE IN CHARGE OF TAT :</strong>
            {{ container.deductible_charge_TAT || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            style="width: 50%"
            ><strong>CURRENCY :</strong>
            {{ container.currency_indemnisation || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - container.estimate.length + 'px',
              marginTop: 20 - container.estimate.length + 'px',
            }"
            style="width: 45%"
            ><strong>INDEMNIFICATION DATE :</strong>
            {{ container.Indemnification_date || "Empty" }}</span
          >
        </div>
        <div class="html2pdf__page-break"></div>
      </div>

      <div
        class="involved-parties"
        v-for="vessel in claim.vessels"
        :key="vessel.id"
      >
        <span class="logo">
          <img src="@/assets/TangerAlliance_Cropped.png" alt="TA LOGO" />
        </span>
        <h2
          style="
            display: inline-block;
            width: 50%;
            float: right;
            margin-top: 15px;
            text-align: right;
          "
        >
          <strong>CLAIM : </strong>{{ getClaimSerialNumber(claim) || "Empty" }}
        </h2>
        <h2
          style="
            margin: 13px 11px;
            display: inline-block;
            margin-bottom: 35px;
            width: 100%;
            text-align: center;
            margin-top: -25px;
          "
        >
          <strong>SUBJECT : </strong>
          {{ vessel.shipping_line.name || "Empty" }} vessel -
          {{ vessel.vessel_number || "Empty" }}
        </h2>
        <div class="claim-info">
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>Claim Date :</strong>
            {{ claim.claim_date || "Empty" }}</span
          >

          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>Status :</strong> {{ claim.status || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>REINVOICED :</strong>
            {{ vessel.reinvoiced || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>Incident Date :</strong>
            {{ claim.incident_date || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>CATEGORY :</strong> {{ claim.category }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>Total Estimation :</strong>
            {{ claim.totalEstimation || "Empty" }}</span
          >
        </div>

        <h2
          style="
            margin: 6px 11px -2px 20px;
            margin-top: 38px;
            margin-bottom: 38px;
          "
        >
          Involved Parties :
        </h2>
        <h3
          style="
            margin-bottom: 5px;
            background-color: rgb(11 54 89);
            color: white;
            width: 100%;
          "
        >
          VESSEL :
        </h3>
        <div class="claim-info">
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            style="width: 50%"
            ><strong>VESSEL NAME :</strong>
            {{ vessel.vessel_number || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            style="width: 45%"
            ><strong>STATUS :</strong>
            {{
              vessel.nature_of_damage.id == 0
                ? "Not Damaged"
                : "Damaged" || "Empty"
            }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>SHIPPINE LINE :</strong>
            {{ vessel.shipping_line.name || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>DECLARED :</strong>
            {{
              vessel.date_of_declaration == null
                ? " Undeclared"
                : "Declared" || "Empty"
            }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            style="width: 50%"
            ><strong>REINVOICED :</strong>
            {{ vessel.reinvoiced || "Empty" }}</span
          >

          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>TOTAL ESTIMATION :</strong>
            {{ getTotalToTo(vessel.estimate) || "Empty" }}</span
          >
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            DAMAGE INFORMATION :
          </h4>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            style="width: 90% !important"
            ><strong>CONCERNED DEPARTMENTS :</strong>
            {{
              getThisEquipmentDepartments(
                vessel.concerned_internal_department
              ) || "Empty"
            }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>NATURE OF DAMAGE :</strong>
            {{ vessel.nature_of_damage.name || "Empty" }}</span
          >

          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>CAUSE OF DAMAGE :</strong>
            {{ vessel.cause_damage || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>CAUSED BY :</strong>
            {{ vessel.damage_caused_by || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            v-if="
              vessel.damage_caused_by == 'TangerAlliance' &&
              vessel.TAT_name_persons != null
            "
            ><strong>Name Of TAT people : :</strong>
            <span
              style="display: block"
              v-if="vessel.TAT_name_persons!=null && vessel.TAT_name_persons!=''"
              v-for="person in vessel.TAT_name_persons?.split('|')"
              :key="person"
              cols="12"
              >{{ person }}</span
            >
          </span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            v-if="vessel.damage_caused_by == 'Outsourcer'"
          >
            <strong style="display: block">COMPANY :</strong
            >{{ vessel.companie.name }}
            <strong style="display: block">NAME OF OUTSOURCER PEOPLE :</strong>
            <span
              style="display: block"
              v-if="vessel.outsourcer_persons!=null && vessel.outsourcer_persons!=''"
              v-for="person in vessel.outsourcer_persons?.split('|')"
              :key="person"
              cols="12"
              >{{ person }}</span
            >
          </span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            v-if="vessel.damage_caused_by == 'Thirdparty'"
          >
            <strong>THIRDPARTY COMPANY :</strong
            >{{ vessel.thirdparty_company_name }}
            <strong style="display: block">THIRDPARTY ACTIVITY :</strong
            >{{ vessel.thirdparty_Activity_comments }}
            <strong style="display: block">NAME OF THIRDPARTY PEOPLE :</strong>
            <span
              style="display: block"
              v-if="vessel.thirdparty_persons!=null && vessel.thirdparty_persons!=''"
              v-for="person in vessel.thirdparty_persons?.split('|')"
              :key="person"
              cols="12"
              >{{ person }}</span
            >
          </span>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            style="width: 100% !important"
            ><strong>DAMAGE DESCRIPTION :</strong>
            {{ vessel.damage_description || "Empty" }}</span
          >
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            ESTIMATIONS INFORMATION :
            <span
              style="
                font-style: italic;
                font-size: 13px;
                font-weight: 100;
                color: #ffffff5e;
              "
              v-if="vessel.estimate.length <= 0"
              >Empty</span
            >
          </h4>
          <table style="width: 100%" v-if="vessel.estimate.length > 0">
            <tr>
              <th style="width: 25%">Name</th>
              <th style="width: 25%">value</th>
              <th style="width: 10%">Currency</th>
              <th style="width: 15%">Exchange rate</th>
              <th style="width: 25%">Valuation value</th>
            </tr>
          </table>
          <table
            style="width: 100%"
            v-for="oneEstimate in vessel.estimate"
            :key="oneEstimate.id"
            v-if="vessel.estimate.length > 0"
          >
            <tr
              v-for="Oneother_valuation in oneEstimate.other_valuation"
              :key="Oneother_valuation.id"
            >
              <td style="width: 25%; border: 1px solid black">
                <strong>
                  {{ Oneother_valuation.name }}
                </strong>
              </td>
              <td style="width: 25%; border: 1px solid black">
                {{ Math.floor(Oneother_valuation.value * 100) / 100 }}
              </td>
              <td style="width: 10%; border: 1px solid black">
                {{ Oneother_valuation.currency }}
              </td>
              <td style="width: 15%; border: 1px solid black">
                {{ Oneother_valuation.taux_change }}
              </td>
              <td style="width: 25%; border: 1px solid black">
                {{ Math.floor(Oneother_valuation.value_valuation * 100) / 100 }}
              </td>
            </tr>
            <tr style="width: 100%; margin-left: 17px">
              <td style="width: 25%"></td>
              <td style="width: 25%"></td>
              <td style="width: 10%"></td>
              <td style="width: 15%">Total estimation :</td>
              <td style="width: 25%">
                {{ Math.floor(getTotal(oneEstimate) * 100) / 100 }} (€)
              </td>
            </tr>
          </table>
          <table style="width: 100%" v-if="vessel.estimate.length > 0">
            <tr>
              <td style="width: 25%"></td>
              <td style="width: 25%"></td>
              <td style="width: 15%"></td>
              <td style="width: 10%"><strong>Total : </strong></td>
              <td style="width: 25%">
                <strong>{{ getTotalToTo(vessel.estimate) }} </strong>
              </td>
            </tr>
          </table>
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            REINVOICED
          </h4>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>AMOUNT :</strong>
            {{ Math.floor(vessel.amount * 100) / 100 || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>CURRENCY :</strong> {{ vessel.currency || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>COMMENT :</strong>
            {{ vessel.comment_third_party || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>REINVOICED :</strong>
            {{ vessel.reinvoiced || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>REIMBURSMENT DATE :</strong>
            {{ vessel.date_of_reimbursement || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            v-if="vessel.reinvoiced == 'REINVOICED'"
            ><strong>REIMBURSMENT DATE :</strong>
            {{ vessel.Invoice_number || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            v-if="vessel.reinvoiced == 'REINVOICED'"
            ><strong>REIMBURSMENT AMOUNT :</strong>
            {{ vessel.reimbursed_amount || "Empty" }}</span
          >
          <h4
            style="
              margin-bottom: 5px;
              background-color: rgb(11 54 89);
              color: white;
              width: 100%;
              padding-left: 5px;
            "
          >
            INSURANCE DECLARATION & FOLLOW UP
          </h4>
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>DECLARATION DATE :</strong>
            {{ vessel.date_of_declaration || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>DATE OF FEEDBACK :</strong>
            {{ vessel.date_of_feedback || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            ><strong>COMMENT :</strong>
            {{ vessel.Indemnification_of_insurer || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            style="width: 50%"
            ><strong>INDEMNIFICATION OF THE INSURER :</strong>
            {{ vessel.Indemnification_of_insurer || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            style="width: 45%"
            ><strong>DEDUCTIBLE IN CHARGE OF TAT :</strong>
            {{ vessel.deductible_charge_TAT || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            style="width: 50%"
            ><strong>CURRENCY :</strong>
            {{ vessel.currency_indemnisation || "Empty" }}</span
          >
          <span
            class="claimHeader"
            :style="{
              marginBottom: 20 - vessel.estimate.length + 'px',
              marginTop: 20 - vessel.estimate.length + 'px',
            }"
            style="width: 45%"
            ><strong>INDEMNIFICATION DATE :</strong>
            {{ vessel.Indemnification_date || "Empty" }}</span
          >
        </div>
        <div class="html2pdf__page-break"></div>
      </div>
    </div>
  </span>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
import Equipment from "../Equipment/Equipment.vue";
export default {
  props: ["claims"],
  data() {
    return {
      claimObject: {},
    };
  },
  mounted() {
    console.log(this.selectedClaimToShow);
    this.setDepartementsAction().then((e) => {});
  },
  computed: {
    ...mapGetters(["getdepartements", "getCLAIMS_TO_DOWNLOAD"]),
    claimHeaderClass() {
      let marginY = 20;
      const totalEstimate =
        this.estimateAutomobile + this.estimateContainer + this.estimateVessel;

      if (totalEstimate > 0) {
        marginY -= totalEstimate;
        if (marginY < 0) marginY = 0; // Ensure margin does not go negative
      }

      return {
        "claim-header": true,
        "adjusted-margin": totalEstimate > 0,
        [`margin-y-${marginY}`]: true,
      };
    },
  },
  methods: {
    ...mapActions(["setDepartementsAction"]),
    getThisEquipmentDepartments(departments_) {
      if (departments_ == null) return "Empty";
      console.log("departments_", departments_);
      let departments = departments_.split("|");
      let deps = "";
      for (let index = 0; index < departments.length; index++) {
        for (let index2 = 0; index2 < this.getdepartements.length; index2++) {
          if (departments[index] == this.getdepartements[index2].id) {
            deps += this.getdepartements[index2].name + ", ";
          }
        }
      }
      return deps;
    },
    getTotal(estimation) {
      let total = 0;
      for (let index = 0; index < estimation.other_valuation.length; index++) {
        const element = estimation.other_valuation[index];
        total += element.value_valuation;
      }
      return total;
    },
    getTotalToTo(estimations) {
      let total = 0;
      for (let index = 0; index < estimations.length; index++) {
        total += this.getTotal(estimations[index]);
      }
      return total + " (€)";
    },
    getClaimSerialNumber(item) {
      if (item.incident_date)
        return (
          item.id.toString().padStart(4, "0") +
          "/" +
          item.type +
          "/" +
          item.incident_date.split("/")[2]
        );
      else return item.id.toString().padStart(4, "0") + "/" + item.type + "/";
    },
  },
};
</script>

<style>
.claim-details {
  font-family: Arial, sans-serif;
}

.claimHeader {
  width: 30%;
  display: inline-block;
  margin-left: 19px;
  margin: 3px 0;
}
.involved-parties {
}
td {
  text-align: center;
}
.logo {
  display: inline-block;
  margin-left: 17px;
  img {
    height: 70px;
    width: 167px;
    margin-bottom: 17px;
    margin-top: 3px;
  }
}
</style>
