<template>
  <v-row>
    <v-col cols="10" class="claimHeader">
      <v-row>
        <v-col cols="6"
          ><strong>Claim : {{ selectedClaimToShow.claimSerial }}</strong></v-col
        >
        <v-col cols="6"
          ><strong>Claim Date:</strong> {{ claim.claim_date || "N/A" }}</v-col
        >
        <v-col cols="6"><strong>Status:</strong> {{ claim.status }}</v-col>
        <v-col cols="6"
          ><strong>Incident Date:</strong> {{ claim.incident_date }}</v-col
        >
        <v-col cols="6"><strong>Subject:</strong> {{ claim.category }}</v-col>
        <v-col cols="6"
          ><strong>Total Estimation:</strong> {{ claim.totalEstimation }}</v-col
        >
      </v-row>
    </v-col>

    <v-col cols="10" style="margin: 0 auto">
      <v-expansion-panels class="ma-0 pa-0">
        <v-expansion-panel class="ma-0 pa-0">
          <v-expansion-panel-header
            class="white--text font-weight-bold equipHeader"
            toggle-icon-color="white"
          >
            EQUIPMENT ({{ claim.equipments.length }})
          </v-expansion-panel-header>
          <v-expansion-panel-content class="claimsDetailPanelContent">
            <v-col cols="12" style="margin: 0 auto">
              <template>
                <v-data-table
                  :headers="equipmentHeaders"
                  :items="claim.equipments"
                  :item-key="'id'"
                  show-expand
                  :single-expand="false"
                  class="claimDetailsTable"
                  expand
                >
                  <template v-slot:item="{ item, isExpanded, expand }">
                    <tr
                      class="claims_tr"
                      @click="expand(!isExpanded)"
                      style="cursor: pointer; z-index: 0"
                    >
                      <td class="cursor" style="z-index: 1">
                        <v-icon @click.stop="expand(!isExpanded)">{{
                          isExpanded ? "mdi-chevron-up" : "mdi-chevron-down"
                        }}</v-icon>
                      </td>
                      <td class="cursor">
                        {{ item.type_of_equipment?.name }}
                      </td>
                      <td class="cursor">
                        {{ item.brand?.name }}
                      </td>
                      <td class="cursor">
                        {{ item.matricule?.id_equipment }}
                      </td>
                      <td class="cursor">
                        {{ item.matricule?.matricule }}
                      </td>
                      <td class="cursor">
                        {{ item.department?.name }}
                      </td>
                      <td class="cursor">
                        <v-chip
                          color="#4CAF50"
                          v-if="item.nature_of_damage.id == 0"
                          class="white--text cursor shipWhite"
                        >
                          Not Damaged
                        </v-chip>
                        <v-chip color="#FF5722" v-else class="shipWhite">
                          Damaged
                        </v-chip>
                      </td>
                      <td class="cursor">
                        {{ getTotalToTo(item.estimate) }}
                      </td>
                      <td class="cursor">
                        <v-chip
                          color="#f54 "
                          v-if="item.date_of_declaration == null"
                          class="white--text cursor shipWhite"
                        >
                          Undeclared
                        </v-chip>
                        <v-chip
                          color="#76ba99"
                          v-else
                          class="white--text cursor shipWhite"
                        >
                          Declared
                        </v-chip>
                      </td>
                      <td class="cursor shipWhite">
                        <v-chip
                          color="#f54 "
                          v-if="item.reinvoiced == null"
                          class="white--text cursor shipWhite"
                        >
                          None
                        </v-chip>
                        <v-chip
                          color="rgb(255, 152, 0)"
                          v-else
                          class="white--text cursor shipWhite"
                        >
                          {{ item.reinvoiced }}
                        </v-chip>
                      </td>
                    </tr>
                  </template>
                  <template v-slot:expanded-item="{ item }">
                    <td
                      colspan="12"
                      class="ma-0 pa-0 claimsDetailPanelContentPanelCenter"
                    >
                      <v-expansion-panels class="pa-0">
                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="red white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            DAMAGE
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row>
                              <v-col cols="12" class="mt-4">
                                <span class="detailTitle"
                                  >Concerned departments :
                                </span>
                                {{
                                  getThisEquipmentDepartments(
                                    item.concerned_internal_department
                                  )
                                }}
                              </v-col>
                              <v-divider
                                inset
                                style="margin-left: 23px"
                              ></v-divider>
                              <v-col cols="12">
                                <v-row>
                                  <v-col cols="6">
                                    <div class="detailTitle">
                                      Nature of damage :
                                    </div>
                                    {{ item.nature_of_damage.name }}
                                  </v-col>
                                  <v-col cols="6">
                                    <div class="detailTitle">
                                      Damage description :
                                    </div>
                                    {{ item.damage_description }}</v-col
                                  >
                                </v-row>
                              </v-col>
                              <v-divider
                                inset
                                style="margin-left: 23px"
                              ></v-divider>
                              <v-col cols="12">
                                <v-row>
                                  <v-col cols="6">
                                    <div class="detailTitle">
                                      Cause of damage :
                                    </div>
                                    {{ item.cause_damage }}
                                  </v-col>
                                  <v-col cols="6">
                                    <v-row>
                                      <v-col cols="3">
                                        <div class="detailTitle">
                                          Caused by :
                                        </div>
                                        {{ item.damage_caused_by }}</v-col
                                      >
                                      <v-col
                                        v-if="
                                          item.damage_caused_by ==
                                            'TangerAlliance' &&
                                          item.TAT_name_persons != null
                                        "
                                        cols="9"
                                      >
                                        <div class="detailTitle">
                                          Name Of TAT people :
                                        </div>

                                        <span
                                          style="display: block"
                                          v-for="person in item.TAT_name_persons.split(
                                            '|'
                                          )"
                                          :key="person"
                                          cols="12"
                                          >{{ person }}</span
                                        >
                                      </v-col>
                                      <v-col
                                        cols="9"
                                        v-if="
                                          item.damage_caused_by == 'Outsourcer'
                                        "
                                      >
                                        <v-row>
                                          <v-col cols="4">
                                            <div class="detailTitle">
                                              Company :
                                            </div>
                                            {{ item.companie.name }}
                                          </v-col>
                                          <v-col
                                            v-if="
                                              item.outsourcer_persons != null
                                            "
                                            cols="9"
                                          >
                                            <div class="detailTitle">
                                              Name Of Outsourcer people :
                                            </div>

                                            <span
                                              style="display: block"
                                              v-for="person in item.outsourcer_persons.split(
                                                '|'
                                              )"
                                              :key="person"
                                              cols="12"
                                              >{{ person }}</span
                                            >
                                          </v-col>
                                        </v-row>
                                      </v-col>
                                      <v-col
                                        cols="9"
                                        v-if="
                                          item.damage_caused_by == 'Thirdparty'
                                        "
                                      >
                                        <v-row>
                                          <v-col cols="4">
                                            <div class="detailTitle">
                                              Third party company :
                                            </div>
                                            {{ item.thirdparty_company_name }}
                                          </v-col>
                                          <v-col cols="3">
                                            <div class="detailTitle">
                                              Third party activity :
                                            </div>
                                            {{
                                              item.thirdparty_Activity_comments
                                            }}
                                          </v-col>
                                          <v-col
                                            v-if="
                                              item.thirdparty_persons != null
                                            "
                                            cols="6"
                                          >
                                            <div class="detailTitle">
                                              Name Of Thirdparty people :
                                            </div>

                                            <span
                                              style="display: block"
                                              v-for="person in item.thirdparty_persons.split(
                                                '|'
                                              )"
                                              :key="person"
                                              cols="12"
                                              >{{ person }}</span
                                            >
                                          </v-col>
                                        </v-row>
                                      </v-col>
                                    </v-row>
                                  </v-col>
                                </v-row>
                              </v-col>
                            </v-row></v-expansion-panel-content
                          >
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="amber white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            ESTIMATION
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row class="py-4">
                              <v-col
                                cols="12"
                                v-for="oneEstimate in item.estimate"
                                :key="oneEstimate.id"
                              >
                                <v-row>
                                  <v-col
                                    cols="12"
                                    v-for="Oneother_valuation in oneEstimate.other_valuation"
                                    :key="Oneother_valuation.id"
                                  >
                                    <v-row>
                                      <v-col cols="2"
                                        ><div class="detailTitle">Name :</div>
                                        {{ Oneother_valuation.name }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">
                                          Currency :
                                        </div>
                                        {{ Oneother_valuation.currency }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">Value :</div>
                                        {{
                                          Math.floor(
                                            Oneother_valuation.value * 100
                                          ) / 100
                                        }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">
                                          Exchange rate :
                                        </div>
                                        {{
                                          Oneother_valuation.taux_change
                                        }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">
                                          Valuation value :
                                        </div>
                                        {{
                                          Math.floor(
                                            Oneother_valuation.value_valuation *
                                              100
                                          ) / 100
                                        }}</v-col
                                      >
                                    </v-row>
                                  </v-col>
                                  <v-col cols="2"></v-col>
                                  <v-col cols="2"></v-col>
                                  <v-col cols="2"></v-col>
                                  <v-col
                                    cols="4"
                                    style="
                                      font-size: 15px;
                                      text-decoration: underline;
                                      display: inline-block;
                                    "
                                  >
                                    <div
                                      class="detailTitle"
                                      style="
                                        font-size: 15px;
                                        text-decoration: underline;
                                        display: inline-block;
                                      "
                                    >
                                      Created at :
                                    </div>
                                    {{ oneEstimate.created_at }}
                                  </v-col>
                                  <v-col
                                    cols="2"
                                    style="
                                      font-size: 15px;
                                      text-decoration: underline;
                                      display: inline-block;
                                    "
                                  >
                                    <div
                                      class="detailTitle"
                                      style="
                                        font-size: 15px;
                                        text-decoration: underline;
                                        display: inline-block;
                                      "
                                    >
                                      Total estimation :
                                    </div>
                                    {{
                                      Math.floor(getTotal(oneEstimate) * 100) /
                                      100
                                    }}
                                    (€)
                                  </v-col>
                                </v-row>
                                <v-divider
                                  inset
                                  style="margin-left: 23px"
                                ></v-divider>
                              </v-col> </v-row
                          ></v-expansion-panel-content>
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="orange white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            REINVOICED
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Amount : </span>
                                {{ Math.floor(item.amount * 100) / 100 }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Currency : </span>
                                {{ item.currency }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Comment : </span>
                                {{ item.comment_third_party }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle">Reinvoiced : </span>
                                {{ item.reinvoiced }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Reimbursement date :
                                </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                              <v-col
                                cols="6"
                                class="mt-4"
                                v-if="item.reinvoiced == 'REINVOICED'"
                              >
                                <span class="detailTitle"
                                  >Invoice number :
                                </span>
                                {{ item.Invoice_number }}
                              </v-col>
                              <v-col
                                cols="6"
                                class="mt-4"
                                v-if="item.reinvoiced == 'REINVOICED'"
                              >
                                <span class="detailTitle"
                                  >Reimbursed amount :
                                </span>
                                {{ item.reimbursed_amount }}
                              </v-col>
                            </v-row></v-expansion-panel-content
                          >
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="teal white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            INSURANCE DECLARATION & FOLLOW UP
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Declaration date :
                                </span>
                                {{ item.date_of_declaration }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Date of feedback :
                                </span>
                                {{ item.date_of_feedback }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Comment : </span>
                                {{ item.Indemnification_of_insurer }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification of the insurer :
                                </span>
                                {{ item.Indemnification_of_insurer }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle">Currency : </span>
                                {{ item.currency_indemnisation }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification Date :
                                </span>
                                {{ item.Indemnification_date }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Deductible in charge of TAT :
                                </span>
                                {{ item.deductible_charge_TAT }}
                              </v-col>
                            </v-row>
                          </v-expansion-panel-content>
                        </v-expansion-panel>
                      </v-expansion-panels>
                    </td>
                  </template>
                </v-data-table>
              </template>
            </v-col>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-col>
    <v-col cols="10" style="margin: 0 auto">
      <v-expansion-panels class="ma-0 pa-0">
        <v-expansion-panel class="ma-0 pa-0">
          <v-expansion-panel-header
            class="white--text font-weight-bold equipHeader"
            toggle-icon-color="white"
          >
            AUTOMOBILES ({{ claim.automobiles.length }})
          </v-expansion-panel-header>
          <v-expansion-panel-content class="claimsDetailPanelContent">
            <v-col cols="12" style="margin: 0 auto">
              <template>
                <v-data-table
                  :headers="equipmentHeaders"
                  :items="claim.automobiles"
                  :item-key="'id'"
                  show-expand
                  :single-expand="false"
                  class="claimDetailsTable"
                  expand
                >
                  <template v-slot:item="{ item, isExpanded, expand }">
                    <tr
                      class="claims_tr"
                      @click="expand(!isExpanded)"
                      style="cursor: pointer; z-index: 0"
                    >
                      <td class="cursor" style="z-index: 1">
                        <v-icon @click.stop="expand(!isExpanded)">{{
                          isExpanded ? "mdi-chevron-up" : "mdi-chevron-down"
                        }}</v-icon>
                      </td>
                      <td class="cursor">
                        {{ item.type_of_equipment?.name }}
                      </td>
                      <td class="cursor">
                        {{ item.brand?.name }}
                      </td>
                      <td class="cursor">
                        {{ item.matricule?.id_equipment }}
                      </td>
                      <td class="cursor">
                        {{ item.matricule?.matricule }}
                      </td>
                      <td class="cursor">
                        {{ item.department?.name }}
                      </td>
                      <td class="cursor">
                        <v-chip
                          color="#4CAF50"
                          v-if="item.nature_of_damage.id == 0"
                          class="white--text cursor shipWhite"
                        >
                          Not Damaged
                        </v-chip>
                        <v-chip color="#FF5722" v-else class="shipWhite">
                          Damaged
                        </v-chip>
                      </td>
                      <td class="cursor">
                        {{ getTotalToTo(item.estimate) }}
                      </td>
                      <td class="cursor">
                        <v-chip
                          color="#f54 "
                          v-if="item.date_of_declaration == null"
                          class="white--text cursor shipWhite"
                        >
                          Undeclared
                        </v-chip>
                        <v-chip
                          color="#76ba99"
                          v-else
                          class="white--text cursor shipWhite"
                        >
                          Declared
                        </v-chip>
                      </td>
                      <td class="cursor shipWhite">
                        <v-chip
                          color="#f54 "
                          v-if="item.reinvoiced == null"
                          class="white--text cursor shipWhite"
                        >
                          None
                        </v-chip>
                        <v-chip
                          color="rgb(255, 152, 0)"
                          v-else
                          class="white--text cursor shipWhite"
                        >
                          {{ item.reinvoiced }}
                        </v-chip>
                      </td>
                    </tr>
                  </template>
                  <template v-slot:expanded-item="{ item }">
                    <td
                      colspan="12"
                      class="ma-0 pa-0 claimsDetailPanelContentPanelCenter"
                    >
                      <v-expansion-panels class="pa-0">
                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="red white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            DAMAGE
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row>
                              <v-col cols="12" class="mt-4">
                                <span class="detailTitle"
                                  >Concerned departments :
                                </span>
                                {{
                                  getThisEquipmentDepartments(
                                    item.concerned_internal_department
                                  )
                                }}
                              </v-col>
                              <v-divider
                                inset
                                style="margin-left: 23px"
                              ></v-divider>
                              <v-col cols="12">
                                <v-row>
                                  <v-col cols="6">
                                    <div class="detailTitle">
                                      Nature of damage :
                                    </div>
                                    {{ item.nature_of_damage.name }}
                                  </v-col>
                                  <v-col cols="6">
                                    <div class="detailTitle">
                                      Damage description :
                                    </div>
                                    {{ item.damage_description }}</v-col
                                  >
                                </v-row>
                              </v-col>
                              <v-divider
                                inset
                                style="margin-left: 23px"
                              ></v-divider>
                              <v-col cols="12">
                                <v-row>
                                  <v-col cols="6">
                                    <div class="detailTitle">
                                      Cause of damage :
                                    </div>
                                    {{ item.cause_damage }}
                                  </v-col>
                                  <v-col cols="6">
                                    <v-row>
                                      <v-col cols="3">
                                        <div class="detailTitle">
                                          Caused by :
                                        </div>
                                        {{ item.damage_caused_by }}</v-col
                                      >
                                      <v-col
                                        v-if="
                                          item.damage_caused_by ==
                                            'TangerAlliance' &&
                                          item.TAT_name_persons != null
                                        "
                                        cols="9"
                                      >
                                        <div class="detailTitle">
                                          Name Of TAT people :
                                        </div>

                                        <span
                                          style="display: block"
                                          v-for="person in item.TAT_name_persons.split(
                                            '|'
                                          )"
                                          :key="person"
                                          cols="12"
                                          >{{ person }}</span
                                        >
                                      </v-col>
                                      <v-col
                                        cols="9"
                                        v-if="
                                          item.damage_caused_by == 'Outsourcer'
                                        "
                                      >
                                        <v-row>
                                          <v-col cols="4">
                                            <div class="detailTitle">
                                              Company :
                                            </div>
                                            {{ item.companie.name }}
                                          </v-col>
                                          <v-col
                                            v-if="
                                              item.outsourcer_persons != null
                                            "
                                            cols="9"
                                          >
                                            <div class="detailTitle">
                                              Name Of Outsourcer people :
                                            </div>

                                            <span
                                              style="display: block"
                                              v-for="person in item.outsourcer_persons.split(
                                                '|'
                                              )"
                                              :key="person"
                                              cols="12"
                                              >{{ person }}</span
                                            >
                                          </v-col>
                                        </v-row>
                                      </v-col>
                                      <v-col
                                        cols="9"
                                        v-if="
                                          item.damage_caused_by == 'Thirdparty'
                                        "
                                      >
                                        <v-row>
                                          <v-col cols="4">
                                            <div class="detailTitle">
                                              Third party company :
                                            </div>
                                            {{ item.thirdparty_company_name }}
                                          </v-col>
                                          <v-col cols="3">
                                            <div class="detailTitle">
                                              Third party activity :
                                            </div>
                                            {{
                                              item.thirdparty_Activity_comments
                                            }}
                                          </v-col>
                                          <v-col
                                            v-if="
                                              item.thirdparty_persons != null
                                            "
                                            cols="6"
                                          >
                                            <div class="detailTitle">
                                              Name Of Thirdparty people :
                                            </div>

                                            <span
                                              style="display: block"
                                              v-for="person in item.thirdparty_persons.split(
                                                '|'
                                              )"
                                              :key="person"
                                              cols="12"
                                              >{{ person }}</span
                                            >
                                          </v-col>
                                        </v-row>
                                      </v-col>
                                    </v-row>
                                  </v-col>
                                </v-row>
                              </v-col>
                            </v-row></v-expansion-panel-content
                          >
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="amber white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            ESTIMATION
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row class="py-4">
                              <v-col
                                cols="12"
                                v-for="oneEstimate in item.estimate"
                                :key="oneEstimate.id"
                              >
                                <v-row>
                                  <v-col
                                    cols="12"
                                    v-for="Oneother_valuation in oneEstimate.other_valuation"
                                    :key="Oneother_valuation.id"
                                  >
                                    <v-row>
                                      <v-col cols="2"
                                        ><div class="detailTitle">Name :</div>
                                        {{ Oneother_valuation.name }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">
                                          Currency :
                                        </div>
                                        {{ Oneother_valuation.currency }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">Value :</div>
                                        {{
                                          Math.floor(
                                            Oneother_valuation.value * 100
                                          ) / 100
                                        }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">
                                          Exchange rate :
                                        </div>
                                        {{
                                          Oneother_valuation.taux_change
                                        }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">
                                          Valuation value :
                                        </div>
                                        {{
                                          Math.floor(
                                            Oneother_valuation.value_valuation *
                                              100
                                          ) / 100
                                        }}</v-col
                                      >
                                    </v-row>
                                  </v-col>
                                  <v-col cols="2"></v-col>
                                  <v-col cols="2"></v-col>
                                  <v-col cols="2"></v-col>
                                  <v-col
                                    cols="4"
                                    style="
                                      font-size: 15px;
                                      text-decoration: underline;
                                      display: inline-block;
                                    "
                                  >
                                    <div
                                      class="detailTitle"
                                      style="
                                        font-size: 15px;
                                        text-decoration: underline;
                                        display: inline-block;
                                      "
                                    >
                                      Created at :
                                    </div>
                                    {{ oneEstimate.created_at }}
                                  </v-col>
                                  <v-col
                                    cols="2"
                                    style="
                                      font-size: 15px;
                                      text-decoration: underline;
                                      display: inline-block;
                                    "
                                  >
                                    <div
                                      class="detailTitle"
                                      style="
                                        font-size: 15px;
                                        text-decoration: underline;
                                        display: inline-block;
                                      "
                                    >
                                      Total estimation :
                                    </div>
                                    {{
                                      Math.floor(getTotal(oneEstimate) * 100) /
                                      100
                                    }}
                                    (€)
                                  </v-col>
                                </v-row>
                                <v-divider
                                  inset
                                  style="margin-left: 23px"
                                ></v-divider>
                              </v-col> </v-row
                          ></v-expansion-panel-content>
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="orange white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            REINVOICED
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Amount : </span>
                                {{ Math.floor(item.amount * 100) / 100 }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Currency : </span>
                                {{ item.currency }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Comment : </span>
                                {{ item.comment_third_party }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle">Reinvoiced : </span>
                                {{ item.reinvoiced }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Reimbursement date :
                                </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                              <v-col
                                cols="6"
                                class="mt-4"
                                v-if="item.reinvoiced == 'REINVOICED'"
                              >
                                <span class="detailTitle"
                                  >Invoice number :
                                </span>
                                {{ item.Invoice_number }}
                              </v-col>
                              <v-col
                                cols="6"
                                class="mt-4"
                                v-if="item.reinvoiced == 'REINVOICED'"
                              >
                                <span class="detailTitle"
                                  >Reimbursed amount :
                                </span>
                                {{ item.reimbursed_amount }}
                              </v-col>
                            </v-row></v-expansion-panel-content
                          >
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="teal white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            INSURANCE DECLARATION & FOLLOW UP
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Declaration date :
                                </span>
                                {{ item.date_of_declaration }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Date of feedback :
                                </span>
                                {{ item.date_of_feedback }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Comment : </span>
                                {{ item.Indemnification_of_insurer }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification of the insurer :
                                </span>
                                {{ item.Indemnification_of_insurer }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle">Currency : </span>
                                {{ item.currency_indemnisation }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification Date :
                                </span>
                                {{ item.Indemnification_date }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Deductible in charge of TAT :
                                </span>
                                {{ item.deductible_charge_TAT }}
                              </v-col>
                            </v-row>
                          </v-expansion-panel-content>
                        </v-expansion-panel>
                      </v-expansion-panels>
                    </td>
                  </template>
                </v-data-table>
              </template>
            </v-col>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-col>
    <v-col cols="10" style="margin: 0 auto">
      <v-expansion-panels class="ma-0 pa-0">
        <v-expansion-panel class="ma-0 pa-0">
          <v-expansion-panel-header
            class="white--text font-weight-bold equipHeader"
            toggle-icon-color="white"
          >
            CONTAINERS ({{ claim.containers.length }})
          </v-expansion-panel-header>
          <v-expansion-panel-content class="claimsDetailPanelContent">
            <v-col cols="12" style="margin: 0 auto">
              <template>
                <v-data-table
                  :headers="containerHeaders"
                  :items="claim.containers"
                  :item-key="'id'"
                  show-expand
                  :single-expand="false"
                  class="claimDetailsTable"
                  expand
                >
                  <template v-slot:item="{ item, isExpanded, expand }">
                    <tr
                      class="claims_tr"
                      @click="expand(!isExpanded)"
                      style="cursor: pointer; z-index: 0"
                    >
                      <td class="cursor" style="z-index: 1">
                        <v-icon @click.stop="expand(!isExpanded)">{{
                          isExpanded ? "mdi-chevron-up" : "mdi-chevron-down"
                        }}</v-icon>
                      </td>
                      <td class="cursor">
                        {{ item.containerID }}
                      </td>
                      <td class="cursor">
                        {{ item.shipping_line.name }}
                      </td>
                      <td class="cursor">
                        <v-chip
                          color=""
                          v-if="item.marchandise == 'NO'"
                          class="white--text cursor shipBlack"
                        >
                          NO
                        </v-chip>
                        <v-chip color="" v-else class="shipBlack"> YES </v-chip>
                      </td>
                      <td class="cursor">
                        {{ item.damage_caused_by }}
                      </td>

                      <td class="cursor">
                        <v-chip
                          color="#4CAF50"
                          v-if="item.nature_of_damage.id == 0"
                          class="white--text cursor shipWhite"
                        >
                          Not Damaged
                        </v-chip>
                        <v-chip color="#FF5722" v-else class="shipWhite">
                          Damaged
                        </v-chip>
                      </td>
                      <td class="cursor">
                        {{ getTotalToTo(item.estimate) }}
                      </td>
                      <td class="cursor">
                        <v-chip
                          color="#f54 "
                          v-if="item.date_of_declaration == null"
                          class="white--text cursor shipWhite"
                        >
                          Undeclared
                        </v-chip>
                        <v-chip
                          color="#76ba99"
                          v-else
                          class="white--text cursor shipWhite"
                        >
                          Declared
                        </v-chip>
                      </td>
                      <td class="cursor shipWhite">
                        <v-chip
                          color="#f54 "
                          v-if="item.reinvoiced == null"
                          class="white--text cursor shipWhite"
                        >
                          None
                        </v-chip>
                        <v-chip
                          color="rgb(255, 152, 0)"
                          v-else
                          class="white--text cursor shipWhite"
                        >
                          {{ item.reinvoiced }}
                        </v-chip>
                      </td>
                    </tr>
                  </template>
                  <template v-slot:expanded-item="{ item }">
                    <td
                      colspan="12"
                      class="ma-0 pa-0 claimsDetailPanelContentPanelCenter"
                    >
                      <v-expansion-panels class="pa-0">
                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="red white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            DAMAGE
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row>
                              <v-col cols="12" class="mt-4">
                                <span class="detailTitle"
                                  >Concerned departments :
                                </span>
                                {{
                                  getThisEquipmentDepartments(
                                    item.concerned_internal_department
                                  )
                                }}
                              </v-col>
                              <v-divider
                                inset
                                style="margin-left: 23px"
                              ></v-divider>
                              <v-col cols="12">
                                <v-row>
                                  <v-col cols="6">
                                    <div class="detailTitle">
                                      Nature of damage :
                                    </div>
                                    {{ item.nature_of_damage.name }}
                                  </v-col>
                                  <v-col cols="6">
                                    <div class="detailTitle">
                                      Damage description :
                                    </div>
                                    {{ item.damage_description }}</v-col
                                  >
                                </v-row>
                              </v-col>
                              <v-divider
                                inset
                                style="margin-left: 23px"
                              ></v-divider>
                              <v-col cols="12">
                                <v-row>
                                  <v-col cols="6">
                                    <div class="detailTitle">
                                      Cause of damage :
                                    </div>
                                    {{ item.cause_damage }}
                                  </v-col>
                                  <v-col cols="6">
                                    <v-row>
                                      <v-col cols="3">
                                        <div class="detailTitle">
                                          Caused by :
                                        </div>
                                        {{ item.damage_caused_by }}</v-col
                                      >
                                      <v-col
                                        v-if="
                                          item.damage_caused_by ==
                                            'TangerAlliance' &&
                                          item.TAT_name_persons != null
                                        "
                                        cols="9"
                                      >
                                        <div class="detailTitle">
                                          Name Of TAT people :
                                        </div>

                                        <span
                                          style="display: block"
                                          v-for="person in item.TAT_name_persons.split(
                                            '|'
                                          )"
                                          :key="person"
                                          cols="12"
                                          >{{ person }}</span
                                        >
                                      </v-col>
                                      <v-col
                                        cols="9"
                                        v-if="
                                          item.damage_caused_by == 'Outsourcer'
                                        "
                                      >
                                        <v-row>
                                          <v-col cols="4">
                                            <div class="detailTitle">
                                              Company :
                                            </div>
                                            {{ item.companie.name }}
                                          </v-col>
                                          <v-col
                                            v-if="
                                              item.outsourcer_persons != null
                                            "
                                            cols="9"
                                          >
                                            <div class="detailTitle">
                                              Name Of Outsourcer people :
                                            </div>

                                            <span
                                              style="display: block"
                                              v-for="person in item.outsourcer_persons.split(
                                                '|'
                                              )"
                                              :key="person"
                                              cols="12"
                                              >{{ person }}</span
                                            >
                                          </v-col>
                                        </v-row>
                                      </v-col>
                                      <v-col
                                        cols="9"
                                        v-if="
                                          item.damage_caused_by == 'Thirdparty'
                                        "
                                      >
                                        <v-row>
                                          <v-col cols="4">
                                            <div class="detailTitle">
                                              Third party company :
                                            </div>
                                            {{ item.thirdparty_company_name }}
                                          </v-col>
                                          <v-col cols="3">
                                            <div class="detailTitle">
                                              Third party activity :
                                            </div>
                                            {{
                                              item.thirdparty_Activity_comments
                                            }}
                                          </v-col>
                                          <v-col
                                            v-if="
                                              item.thirdparty_persons != null
                                            "
                                            cols="6"
                                          >
                                            <div class="detailTitle">
                                              Name Of Thirdparty people :
                                            </div>

                                            <span
                                              style="display: block"
                                              v-for="person in item.thirdparty_persons.split(
                                                '|'
                                              )"
                                              :key="person"
                                              cols="12"
                                              >{{ person }}</span
                                            >
                                          </v-col>
                                        </v-row>
                                      </v-col>
                                    </v-row>
                                  </v-col>
                                </v-row>
                              </v-col>
                            </v-row></v-expansion-panel-content
                          >
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="amber white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            ESTIMATION
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row class="py-4">
                              <v-col
                                cols="12"
                                v-for="oneEstimate in item.estimate"
                                :key="oneEstimate.id"
                              >
                                <v-row>
                                  <v-col
                                    cols="12"
                                    v-for="Oneother_valuation in oneEstimate.other_valuation"
                                    :key="Oneother_valuation.id"
                                  >
                                    <v-row>
                                      <v-col cols="2"
                                        ><div class="detailTitle">Name :</div>
                                        {{ Oneother_valuation.name }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">
                                          Currency :
                                        </div>
                                        {{ Oneother_valuation.currency }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">Value :</div>
                                        {{
                                          Math.floor(
                                            Oneother_valuation.value * 100
                                          ) / 100
                                        }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">
                                          Exchange rate :
                                        </div>
                                        {{
                                          Oneother_valuation.taux_change
                                        }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">
                                          Valuation value :
                                        </div>
                                        {{
                                          Math.floor(
                                            Oneother_valuation.value_valuation *
                                              100
                                          ) / 100
                                        }}</v-col
                                      >
                                    </v-row>
                                  </v-col>
                                  <v-col cols="2"></v-col>
                                  <v-col cols="2"></v-col>
                                  <v-col cols="2"></v-col>
                                  <v-col
                                    cols="4"
                                    style="
                                      font-size: 15px;
                                      text-decoration: underline;
                                      display: inline-block;
                                    "
                                  >
                                    <div
                                      class="detailTitle"
                                      style="
                                        font-size: 15px;
                                        text-decoration: underline;
                                        display: inline-block;
                                      "
                                    >
                                      Created at :
                                    </div>
                                    {{ oneEstimate.created_at }}
                                  </v-col>
                                  <v-col
                                    cols="2"
                                    style="
                                      font-size: 15px;
                                      text-decoration: underline;
                                      display: inline-block;
                                    "
                                  >
                                    <div
                                      class="detailTitle"
                                      style="
                                        font-size: 15px;
                                        text-decoration: underline;
                                        display: inline-block;
                                      "
                                    >
                                      Total estimation :
                                    </div>
                                    {{
                                      Math.floor(getTotal(oneEstimate) * 100) /
                                      100
                                    }}
                                    (€)
                                  </v-col>
                                </v-row>
                                <v-divider
                                  inset
                                  style="margin-left: 23px"
                                ></v-divider>
                              </v-col> </v-row
                          ></v-expansion-panel-content>
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="orange white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            REINVOICED
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Amount : </span>
                                {{ Math.floor(item.amount * 100) / 100 }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Currency : </span>
                                {{ item.currency }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Comment : </span>
                                {{ item.comment_third_party }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle">Reinvoiced : </span>
                                {{ item.reinvoiced }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Reimbursement date :
                                </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                              <v-col
                                cols="6"
                                class="mt-4"
                                v-if="item.reinvoiced == 'REINVOICED'"
                              >
                                <span class="detailTitle"
                                  >Invoice number :
                                </span>
                                {{ item.Invoice_number }}
                              </v-col>
                              <v-col
                                cols="6"
                                class="mt-4"
                                v-if="item.reinvoiced == 'REINVOICED'"
                              >
                                <span class="detailTitle"
                                  >Reimbursed amount :
                                </span>
                                {{ item.reimbursed_amount }}
                              </v-col>
                            </v-row></v-expansion-panel-content
                          >
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="teal white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            INSURANCE DECLARATION & FOLLOW UP
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Declaration date :
                                </span>
                                {{ item.date_of_declaration }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Date of feedback :
                                </span>
                                {{ item.date_of_feedback }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Comment : </span>
                                {{ item.Indemnification_of_insurer }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification of the insurer :
                                </span>
                                {{ item.Indemnification_of_insurer }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle">Currency : </span>
                                {{ item.currency_indemnisation }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification Date :
                                </span>
                                {{ item.Indemnification_date }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Deductible in charge of TAT :
                                </span>
                                {{ item.deductible_charge_TAT }}
                              </v-col>
                            </v-row>
                          </v-expansion-panel-content>
                        </v-expansion-panel>
                      </v-expansion-panels>
                    </td>
                  </template>
                </v-data-table>
              </template>
            </v-col>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-col>

    <v-col cols="10" style="margin: 0 auto">
      <v-expansion-panels class="ma-0 pa-0">
        <v-expansion-panel class="ma-0 pa-0">
          <v-expansion-panel-header
            class="white--text font-weight-bold equipHeader"
            toggle-icon-color="white"
          >
            VESSELS ({{ claim.vessels.length }})
          </v-expansion-panel-header>
          <v-expansion-panel-content class="claimsDetailPanelContent">
            <v-col cols="12" style="margin: 0 auto">
              <template>
                <v-data-table
                  :headers="vesselHeaders"
                  :items="claim.vessels"
                  :item-key="'id'"
                  show-expand
                  :single-expand="false"
                  class="claimDetailsTable"
                  expand
                >
                  <template v-slot:item="{ item, isExpanded, expand }">
                    <tr
                      class="claims_tr"
                      @click="expand(!isExpanded)"
                      style="cursor: pointer; z-index: 0"
                    >
                      <td class="cursor" style="z-index: 1">
                        <v-icon @click.stop="expand(!isExpanded)">{{
                          isExpanded ? "mdi-chevron-up" : "mdi-chevron-down"
                        }}</v-icon>
                      </td>
                      <td class="cursor">
                        {{ item.vessel_number }}
                      </td>
                      <td class="cursor">
                        {{ item.shipping_line.name }}
                      </td>

                      <td class="cursor">
                        {{ item.damage_caused_by }}
                      </td>

                      <td class="cursor">
                        <v-chip
                          color="#4CAF50"
                          v-if="item.nature_of_damage.id == 0"
                          class="white--text cursor shipWhite"
                        >
                          Not Damaged
                        </v-chip>
                        <v-chip color="#FF5722" v-else class="shipWhite">
                          Damaged
                        </v-chip>
                      </td>
                      <td class="cursor">
                        {{ getTotalToTo(item.estimate) }}
                      </td>
                      <td class="cursor">
                        <v-chip
                          color="#f54 "
                          v-if="item.date_of_declaration == null"
                          class="white--text cursor shipWhite"
                        >
                          Undeclared
                        </v-chip>
                        <v-chip
                          color="#76ba99"
                          v-else
                          class="white--text cursor shipWhite"
                        >
                          Declared
                        </v-chip>
                      </td>
                      <td class="cursor shipWhite">
                        <v-chip
                          color="#f54 "
                          v-if="item.reinvoiced == null"
                          class="white--text cursor shipWhite"
                        >
                          None
                        </v-chip>
                        <v-chip
                          color="rgb(255, 152, 0)"
                          v-else
                          class="white--text cursor shipWhite"
                        >
                          {{ item.reinvoiced }}
                        </v-chip>
                      </td>
                    </tr>
                  </template>
                  <template v-slot:expanded-item="{ item }">
                    <td
                      colspan="12"
                      class="ma-0 pa-0 claimsDetailPanelContentPanelCenter"
                    >
                      <v-expansion-panels class="pa-0">
                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="red white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            DAMAGE
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row>
                              <v-col cols="12" class="mt-4">
                                <span class="detailTitle"
                                  >Concerned departments :
                                </span>
                                {{
                                  getThisEquipmentDepartments(
                                    item.concerned_internal_department
                                  )
                                }}
                              </v-col>
                              <v-divider
                                inset
                                style="margin-left: 23px"
                              ></v-divider>
                              <v-col cols="12">
                                <v-row>
                                  <v-col cols="6">
                                    <div class="detailTitle">
                                      Nature of damage :
                                    </div>
                                    {{ item.nature_of_damage.name }}
                                  </v-col>
                                  <v-col cols="6">
                                    <div class="detailTitle">
                                      Damage description :
                                    </div>
                                    {{ item.damage_description }}</v-col
                                  >
                                </v-row>
                              </v-col>
                              <v-divider
                                inset
                                style="margin-left: 23px"
                              ></v-divider>
                              <v-col cols="12">
                                <v-row>
                                  <v-col cols="6">
                                    <div class="detailTitle">
                                      Cause of damage :
                                    </div>
                                    {{ item.cause_damage }}
                                  </v-col>
                                  <v-col cols="6">
                                    <v-row>
                                      <v-col cols="3">
                                        <div class="detailTitle">
                                          Caused by :
                                        </div>
                                        {{ item.damage_caused_by }}</v-col
                                      >
                                      <v-col
                                        v-if="
                                          item.damage_caused_by ==
                                            'TangerAlliance' &&
                                          item.TAT_name_persons != null
                                        "
                                        cols="9"
                                      >
                                        <div class="detailTitle">
                                          Name Of TAT people :
                                        </div>

                                        <span
                                          style="display: block"
                                          v-for="person in item.TAT_name_persons.split(
                                            '|'
                                          )"
                                          :key="person"
                                          cols="12"
                                          >{{ person }}</span
                                        >
                                      </v-col>
                                      <v-col
                                        cols="9"
                                        v-if="
                                          item.damage_caused_by == 'Outsourcer'
                                        "
                                      >
                                        <v-row>
                                          <v-col cols="4">
                                            <div class="detailTitle">
                                              Company :
                                            </div>
                                            {{ item.companie.name }}
                                          </v-col>
                                          <v-col
                                            v-if="
                                              item.outsourcer_persons != null
                                            "
                                            cols="9"
                                          >
                                            <div class="detailTitle">
                                              Name Of Outsourcer people :
                                            </div>

                                            <span
                                              style="display: block"
                                              v-for="person in item.outsourcer_persons.split(
                                                '|'
                                              )"
                                              :key="person"
                                              cols="12"
                                              >{{ person }}</span
                                            >
                                          </v-col>
                                        </v-row>
                                      </v-col>
                                      <v-col
                                        cols="9"
                                        v-if="
                                          item.damage_caused_by == 'Thirdparty'
                                        "
                                      >
                                        <v-row>
                                          <v-col cols="4">
                                            <div class="detailTitle">
                                              Third party company :
                                            </div>
                                            {{ item.thirdparty_company_name }}
                                          </v-col>
                                          <v-col cols="3">
                                            <div class="detailTitle">
                                              Third party activity :
                                            </div>
                                            {{
                                              item.thirdparty_Activity_comments
                                            }}
                                          </v-col>
                                          <v-col
                                            v-if="
                                              item.thirdparty_persons != null
                                            "
                                            cols="6"
                                          >
                                            <div class="detailTitle">
                                              Name Of Thirdparty people :
                                            </div>

                                            <span
                                              style="display: block"
                                              v-for="person in item.thirdparty_persons.split(
                                                '|'
                                              )"
                                              :key="person"
                                              cols="12"
                                              >{{ person }}</span
                                            >
                                          </v-col>
                                        </v-row>
                                      </v-col>
                                    </v-row>
                                  </v-col>
                                </v-row>
                              </v-col>
                            </v-row></v-expansion-panel-content
                          >
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="amber white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            ESTIMATION
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row class="py-4">
                              <v-col
                                cols="12"
                                v-for="oneEstimate in item.estimate"
                                :key="oneEstimate.id"
                              >
                                <v-row>
                                  <v-col
                                    cols="12"
                                    v-for="Oneother_valuation in oneEstimate.other_valuation"
                                    :key="Oneother_valuation.id"
                                  >
                                    <v-row>
                                      <v-col cols="2"
                                        ><div class="detailTitle">Name :</div>
                                        {{ Oneother_valuation.name }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">
                                          Currency :
                                        </div>
                                        {{ Oneother_valuation.currency }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">Value :</div>
                                        {{
                                          Math.floor(
                                            Oneother_valuation.value * 100
                                          ) / 100
                                        }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">
                                          Exchange rate :
                                        </div>
                                        {{
                                          Oneother_valuation.taux_change
                                        }}</v-col
                                      >
                                      <v-col cols="2"
                                        ><div class="detailTitle">
                                          Valuation value :
                                        </div>
                                        {{
                                          Math.floor(
                                            Oneother_valuation.value_valuation *
                                              100
                                          ) / 100
                                        }}</v-col
                                      >
                                    </v-row>
                                  </v-col>
                                  <v-col cols="2"></v-col>
                                  <v-col cols="2"></v-col>
                                  <v-col cols="2"></v-col>
                                  <v-col
                                    cols="4"
                                    style="
                                      font-size: 15px;
                                      text-decoration: underline;
                                      display: inline-block;
                                    "
                                  >
                                    <div
                                      class="detailTitle"
                                      style="
                                        font-size: 15px;
                                        text-decoration: underline;
                                        display: inline-block;
                                      "
                                    >
                                      Created at :
                                    </div>
                                    {{ oneEstimate.created_at }}
                                  </v-col>
                                  <v-col
                                    cols="2"
                                    style="
                                      font-size: 15px;
                                      text-decoration: underline;
                                      display: inline-block;
                                    "
                                  >
                                    <div
                                      class="detailTitle"
                                      style="
                                        font-size: 15px;
                                        text-decoration: underline;
                                        display: inline-block;
                                      "
                                    >
                                      Total estimation :
                                    </div>
                                    {{
                                      Math.floor(getTotal(oneEstimate) * 100) /
                                      100
                                    }}
                                    (€)
                                  </v-col>
                                </v-row>
                                <v-divider
                                  inset
                                  style="margin-left: 23px"
                                ></v-divider>
                              </v-col> </v-row
                          ></v-expansion-panel-content>
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="orange white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            REINVOICED
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Amount : </span>
                                {{ Math.floor(item.amount * 100) / 100 }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Currency : </span>
                                {{ item.currency }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Comment : </span>
                                {{ item.comment_third_party }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle">Reinvoiced : </span>
                                {{ item.reinvoiced }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Reimbursement date :
                                </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                              <v-col
                                cols="6"
                                class="mt-4"
                                v-if="item.reinvoiced == 'REINVOICED'"
                              >
                                <span class="detailTitle"
                                  >Invoice number :
                                </span>
                                {{ item.Invoice_number }}
                              </v-col>
                              <v-col
                                cols="6"
                                class="mt-4"
                                v-if="item.reinvoiced == 'REINVOICED'"
                              >
                                <span class="detailTitle"
                                  >Reimbursed amount :
                                </span>
                                {{ item.reimbursed_amount }}
                              </v-col>
                            </v-row></v-expansion-panel-content
                          >
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="teal white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            INSURANCE DECLARATION & FOLLOW UP
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            ><v-row>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Declaration date :
                                </span>
                                {{ item.date_of_declaration }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Date of feedback :
                                </span>
                                {{ item.date_of_feedback }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Comment : </span>
                                {{ item.Indemnification_of_insurer }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification of the insurer :
                                </span>
                                {{ item.Indemnification_of_insurer }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle">Currency : </span>
                                {{ item.currency_indemnisation }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification Date :
                                </span>
                                {{ item.Indemnification_date }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Deductible in charge of TAT :
                                </span>
                                {{ item.deductible_charge_TAT }}
                              </v-col>
                            </v-row>
                          </v-expansion-panel-content>
                        </v-expansion-panel>
                      </v-expansion-panels>
                    </td>
                  </template>
                </v-data-table>
              </template>
            </v-col>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-col>
  </v-row>
</template>
<script>
import { mapActions, mapGetters } from "vuex";
export default {
  name: "showDetails",
  props: ["selectedClaimToShow"],
  data() {
    return {
      expanded: [],
      claim: {},
      equipmentHeaders: [
        {
          text: "TYPE",

          sortable: true,
          value: "type_of_equipment.name",
        },
        { text: "BRAND", value: "brand.name", sortable: true },
        {
          text: "EQUIPMENT ID",
          value: "matricule.id_equipment",
          sortable: true,
        },
        { text: "MATRICULE", value: "matricule.matricule", sortable: true },
        { text: "DEPARTMENT(OWNER)", value: "department.name", sortable: true },
        { text: "STATUS", value: "matricule.matricule", sortable: true },
        { text: "Total extimation", value: "amount", sortable: true },
        { text: "DECLARED", value: "amount", sortable: true },
        { text: "REINVOICED", value: "amount", sortable: true },
      ],
      containerHeaders: [
        {
          text: "CONTAINER ID",
          sortable: true,
          value: "containerID",
        },
        {
          text: "SHIPPING LINE",
          value: "shipping_line.name",
          sortable: true,
        },
        {
          text: "MARCHANDISE",
          value: "shipping_line.name",
          sortable: true,
        },
        {
          text: "CAUSED BY",
          value: "shipping_line.name",
          sortable: true,
        },
        { text: "STATUS", value: "matricule.matricule", sortable: true },
        { text: "Total extimation", value: "amount", sortable: true },
        { text: "DECLARED", value: "amount", sortable: true },
        { text: "REINVOICED", value: "amount", sortable: true },
      ],
      vesselHeaders: [
        {
          text: "VESSEL ID",
          sortable: true,
          value: "containerID",
        },
        {
          text: "SHIPPING LINE",
          value: "shipping_line.name",
          sortable: true,
        },
        {
          text: "CAUSED BY",
          value: "shipping_line.name",
          sortable: true,
        },
        { text: "STATUS", value: "matricule.matricule", sortable: true },
        { text: "Total extimation", value: "amount", sortable: true },
        { text: "DECLARED", value: "amount", sortable: true },
        { text: "REINVOICED", value: "amount", sortable: true },
      ],
    };
  },

  mounted() {
    console.log(this.selectedClaimToShow);
    this.claim = this.getCLAIMS_TO_DOWNLOAD[0];
    this.setDepartementsAction().then((e) => {
      console.log(this.claim);
    });
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
  },
  computed: {
    ...mapGetters(["getdepartements", "getCLAIMS_TO_DOWNLOAD"]),
  },
};
</script>
<style lang="scss">
.claimDetailsTable * {
  color: black !important;
  td {
    font-weight: bold;
  }
  td {
    border-top: 1px solid black !important;
    border-bottom: 1px solid black !important;
  }
}
.claimHeader {
  margin: 0px auto;
  text-align: left;
  padding-top: 26px;
  /* background: #1f497d; */
  /* color: white; */
  font-size: 15px;
  margin-bottom: 15px;
}
.equipHeader {
  margin: 0px auto;

  text-align: center;
  padding-top: 14px;
  background: #1f497d;
  font-weight: bold;
  color: white;
  letter-spacing: 6px;
  font-size: 15px;
}
.detailTitle {
  font-weight: bold;
}
.shipWhite * {
  color: white !important;
  cursor: pointer;
}
.shipBlack * {
  color: white !important;
  cursor: pointer;
}
</style>
