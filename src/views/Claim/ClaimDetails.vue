<template>
  <v-row>
    <v-col cols="10" class="claimHeader">Claim : 0001/D/2023</v-col>
    <v-col cols="10" style="margin: 0 auto">
      <v-expansion-panels class="ma-0 pa-0">
        <v-expansion-panel class="ma-0 pa-0">
          <v-expansion-panel-header
            class="white--text font-weight-bold equipHeader"
            toggle-icon-color="white"
          >
            EQUIPMENT ({{ claim.equipments.length }})
          </v-expansion-panel-header>
          <v-expansion-panel-content
            style="background: rgb(31 73 125 / 65%); color: white"
          >
            <v-col cols="12" style="margin: 0 auto">
              <template>
                <v-data-table
                  :headers="equipmentHeaders"
                  :items="claim.equipments"
                  :item-key="'id'"
                  show-expand
                  :single-expand="false"
                  class="claimDetailsTable"
                >
                  <template v-slot:expanded-item="{ item }">
                    <td colspan="8" class="ma-0 pa-0">
                      <v-expansion-panels class="ma-0 pa-0">
                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="teal white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            DAMAGE
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            style="background: #009688; color: white"
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
                                            {{ item.outsourcer_company_name }}
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
                            style="background: #ffc107a6; color: black"
                            ><v-row>
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
                            style="background: #ff980091; color: black"
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
                            </v-row></v-expansion-panel-content
                          >
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="red white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            INSURANCE DECLARATION & FOLLOW UP
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            style="background: #f44336c7; color: white"
                            ><v-row>
                              <v-col cols="12" class="mt-4">
                                <span>Insurance declaration : </span>
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Declaration date :
                                </span>
                                {{ Math.floor(item.amount * 100) / 100 }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Date of feedback :
                                </span>
                                {{ item.currency }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Comment : </span>
                                {{ item.comment_third_party }}
                              </v-col>
                              <v-divider
                                inset
                                style="margin-left: 23px"
                              ></v-divider>
                              <v-col cols="12" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnisation :
                                </span>
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification of the insurer :
                                </span>
                                {{ item.reinvoiced }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle">Currency : </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification :
                                </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Deductible in charge of TAT :
                                </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                            </v-row></v-expansion-panel-content
                          >
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
            style="background-color: black"
          >
            AUTOMOBILES ({{ claim.automobiles.length }})
          </v-expansion-panel-header>
          <v-expansion-panel-content
            style="background: #0000008c; color: white"
          >
            <v-col cols="12" style="margin: 0 auto">
              <template>
                <v-data-table
                  :headers="equipmentHeaders"
                  :items="claim.automobiles"
                  :item-key="'id'"
                  show-expand
                  :single-expand="false"
                  class="claimDetailsTable"
                >
                  <template v-slot:expanded-item="{ item }">
                    <td colspan="8" class="ma-0 pa-0">
                      <v-expansion-panels class="ma-0 pa-0">
                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="teal white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            DAMAGE
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            style="background: #009688; color: white"
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
                                            {{ item.outsourcer_company_name }}
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
                            style="background: #ffc107a6; color: black"
                            ><v-row>
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
                            style="background: #ff980091; color: black"
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
                            </v-row></v-expansion-panel-content
                          >
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="red white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            INSURANCE DECLARATION & FOLLOW UP
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            style="background: #f44336c7; color: white"
                            ><v-row>
                              <v-col cols="12" class="mt-4">
                                <span>Insurance declaration : </span>
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Declaration date :
                                </span>
                                {{ Math.floor(item.amount * 100) / 100 }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Date of feedback :
                                </span>
                                {{ item.currency }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Comment : </span>
                                {{ item.comment_third_party }}
                              </v-col>
                              <v-divider
                                inset
                                style="margin-left: 23px"
                              ></v-divider>
                              <v-col cols="12" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnisation :
                                </span>
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification of the insurer :
                                </span>
                                {{ item.reinvoiced }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle">Currency : </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification :
                                </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Deductible in charge of TAT :
                                </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                            </v-row></v-expansion-panel-content
                          >
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
            style="background-color: #4caf50"
          >
            CONTAINERS ({{ claim.containers.length }})
          </v-expansion-panel-header>
          <v-expansion-panel-content
            style="background: rgb(76 175 80 / 32%); color: white"
          >
            <v-col cols="12" style="margin: 0 auto">
              <template>
                <v-data-table
                  :headers="containerHeaders"
                  :items="claim.containers"
                  :item-key="'id'"
                  show-expand
                  :single-expand="false"
                  class="claimDetailsTable"
                >
                  <template v-slot:expanded-item="{ item }">
                    <td colspan="8" class="ma-0 pa-0">
                      <v-expansion-panels class="ma-0 pa-0">
                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="teal white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            DAMAGE
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            style="background: #009688; color: white"
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
                                            {{ item.outsourcer_company_name }}
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
                            style="background: #ffc107a6; color: black"
                            ><v-row>
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
                            style="background: #ff980091; color: black"
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
                            </v-row></v-expansion-panel-content
                          >
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="red white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            INSURANCE DECLARATION & FOLLOW UP
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            style="background: #f44336c7; color: white"
                            ><v-row>
                              <v-col cols="12" class="mt-4">
                                <span>Insurance declaration : </span>
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Declaration date :
                                </span>
                                {{ Math.floor(item.amount * 100) / 100 }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Date of feedback :
                                </span>
                                {{ item.currency }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Comment : </span>
                                {{ item.comment_third_party }}
                              </v-col>
                              <v-divider
                                inset
                                style="margin-left: 23px"
                              ></v-divider>
                              <v-col cols="12" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnisation :
                                </span>
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification of the insurer :
                                </span>
                                {{ item.reinvoiced }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle">Currency : </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification :
                                </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Deductible in charge of TAT :
                                </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                            </v-row></v-expansion-panel-content
                          >
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
            style="background-color: #ffa500"
          >
            VESSELS ({{ claim.vessels.length }})
          </v-expansion-panel-header>
          <v-expansion-panel-content style="background: #ffa500; color: white">
            <v-col cols="12" style="margin: 0 auto">
              <template>
                <v-data-table
                  :headers="containerHeaders"
                  :items="claim.vessels"
                  :item-key="'id'"
                  show-expand
                  :single-expand="false"
                  class="claimDetailsTable"
                >
                  <template v-slot:expanded-item="{ item }">
                    <td colspan="8" class="ma-0 pa-0">
                      <v-expansion-panels class="ma-0 pa-0">
                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="teal white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            DAMAGE
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            style="background: #009688; color: white"
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
                                            {{ item.outsourcer_company_name }}
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
                            style="background: #ffc107a6; color: black"
                            ><v-row>
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
                            style="background: #ff980091; color: black"
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
                            </v-row></v-expansion-panel-content
                          >
                        </v-expansion-panel>

                        <v-expansion-panel class="ma-0 pa-0">
                          <v-expansion-panel-header
                            class="red white--text font-weight-bold"
                            toggle-icon-color="white"
                          >
                            INSURANCE DECLARATION & FOLLOW UP
                          </v-expansion-panel-header>
                          <v-expansion-panel-content
                            style="background: #f44336c7; color: white"
                            ><v-row>
                              <v-col cols="12" class="mt-4">
                                <span>Insurance declaration : </span>
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Declaration date :
                                </span>
                                {{ Math.floor(item.amount * 100) / 100 }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle"
                                  >Date of feedback :
                                </span>
                                {{ item.currency }}
                              </v-col>
                              <v-col cols="4" class="mt-4">
                                <span class="detailTitle">Comment : </span>
                                {{ item.comment_third_party }}
                              </v-col>
                              <v-divider
                                inset
                                style="margin-left: 23px"
                              ></v-divider>
                              <v-col cols="12" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnisation :
                                </span>
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification of the insurer :
                                </span>
                                {{ item.reinvoiced }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle">Currency : </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Indemnification :
                                </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                              <v-col cols="6" class="mt-4">
                                <span class="detailTitle"
                                  >Deductible in charge of TAT :
                                </span>
                                {{ item.date_of_reimbursement }}
                              </v-col>
                            </v-row></v-expansion-panel-content
                          >
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
  data() {
    return {
      expanded: [],
      claim: {
        id: 1,
        claim_date: null,
        incident_date: "01/01/2023",
        ClaimOrIncident: "Incident",
        status: "On progress",
        incident_report: null,
        type: "D",
        created_at: "02/16/2023 16:12",
        updated_at: "10/02/2023 13:16",
        totalEstimation: "209908 (EUR)",
        category: "Vessel - HLC : 055298",
        getEquipmentIds: [
          "Equipment - RTGs : 01",
          "Equipment - Trailers : 27",
          "Equipment - RTGs : 02",
          "Equipment - RTGs : 02",
          "Equipment",
          "Automobile - RTGs : 03",
          "Container - ZIM : ZCSU8929070",
          "Vessel - HLC : 055298",
          "Vessel - HLC : 055298",
        ],
        getDeclare: "Container At : 2023-11-14",
        equipments: [
          {
            id: 1,
            name: null,
            claim_id: 1,
            type_of_equipment_id: 2,
            brand_id: 9,
            companie_id: 1,
            nature_of_damage_id: 2,
            department_id: 4,
            matricule_id: 9,
            concerned_internal_department: "2|1|3|4",
            cause_damage: "Maneuvring skils (new driver ) / Lack of vigilance",
            Liability_letter_number: null,
            amount: 272.51,
            currency: "EUR",
            comment_third_party: null,
            reinvoiced: "PAID",
            currency_Insurance: null,
            Invoice_number: 7,
            date_of_reimbursement: "10/07/2023",
            reimbursed_amount: 272.51,
            date_of_declaration: null,
            date_of_feedback: null,
            comment_Insurance: null,
            Indemnification_of_insurer: null,
            Indemnification_date: null,
            currency_indemnisation: null,
            deductible_charge_TAT: 5000,
            damage_caused_by: "TangerAlliance",
            comment_nature_of_damage: null,
            TAT_name_persons: "az dd|zs sq|ssd",
            outsourcer_company_name: null,
            outsourcer_persons: "BRAHIM HARBAL",
            thirdparty_company_name: null,
            thirdparty_Activity_comments: null,
            thirdparty_persons: null,
            incident_report: null,
            liability_letter: null,
            insurance_declaration: null,
            created_at: "02/16/2023 16:14",
            updated_at: "10/10/2023 13:24",
            major: 0,
            damage_description:
              "While TT 03 (Tailer 03) was stopping in Truck line waiting his turn for load by QC 07 , TT 27 (Tailer 27) coming from backreach hit TT 03 with the trailer as he was trying overtaking him in the adjacent truck line",
            type_of_equipment: {
              id: 2,
              name: "RTGs",
              categorie: "Equipment",
              created_at: "16/02/2023 16:10",
              updated_at: "16/02/2023 16:10",
            },
            brand: {
              id: 9,
              name: "COMATO",
              categorie: "Equipment",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
            estimate: [
              {
                id: 56,
                temporary_or_permanent: "Permanent",
                currency_estimate: "EUR",
                equipment_id: 1,
                container_id: null,
                automobile_id: null,
                vessel_id: null,
                created_at: "18/05/2023 12:14",
                updated_at: "18/05/2023 12:14",
                other_valuation: [
                  {
                    id: 55,
                    name: "Repair",
                    currency: "EUR",
                    value: 70,
                    taux_change: 1,
                    value_valuation: 70,
                    estimate_id: 56,
                    created_at: "2023-05-18T12:14:46.000000Z",
                    updated_at: "2023-05-18T12:14:46.000000Z",
                  },
                ],
              },
              {
                id: 58,
                temporary_or_permanent: "Permanent",
                currency_estimate: "EUR",
                equipment_id: 1,
                container_id: null,
                automobile_id: null,
                vessel_id: null,
                created_at: "18/05/2023 12:18",
                updated_at: "18/05/2023 12:18",
                other_valuation: [
                  {
                    id: 57,
                    name: "Labor cost",
                    currency: "EUR",
                    value: 3.5300000000000002,
                    taux_change: 1,
                    value_valuation: 3.5300000000000002,
                    estimate_id: 58,
                    created_at: "2023-05-18T12:18:51.000000Z",
                    updated_at: "2023-05-18T12:20:30.000000Z",
                  },
                ],
              },
              {
                id: 62,
                temporary_or_permanent: "Permanent",
                currency_estimate: "EUR",
                equipment_id: 1,
                container_id: null,
                automobile_id: null,
                vessel_id: null,
                created_at: "18/05/2023 12:27",
                updated_at: "18/05/2023 12:27",
                other_valuation: [
                  {
                    id: 62,
                    name: "Equipment purchase costs",
                    currency: "EUR",
                    value: 72.81,
                    taux_change: 1,
                    value_valuation: 72.81,
                    estimate_id: 62,
                    created_at: "2023-05-18T12:27:35.000000Z",
                    updated_at: "2023-05-18T12:27:35.000000Z",
                  },
                ],
              },
            ],
            nature_of_damage: {
              id: 2,
              name: "Glass breakage",
              categorie: "Equipment",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
            companie: {
              id: 1,
              name: "TSP",
              categorie: "equipment",
              created_at: "2023-02-16T16:14:39.000000Z",
              updated_at: "2023-02-16T16:14:39.000000Z",
            },
            department: {
              id: 4,
              name: "TECHNICAL",
              email: null,
              created_at: "16/02/2023 16:10",
              updated_at: "16/02/2023 16:10",
            },
            matricule: {
              id: 9,
              id_equipment: "01",
              id_automobile: null,
              matricule: "G2439",
              equipment: "RTGs",
              brand: "KONECRANES",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
          },
          {
            id: 68,
            name: null,
            claim_id: 1,
            type_of_equipment_id: 9,
            brand_id: 6,
            companie_id: null,
            nature_of_damage_id: 2,
            department_id: 4,
            matricule_id: 136,
            concerned_internal_department: null,
            cause_damage: null,
            Liability_letter_number: null,
            amount: null,
            currency: null,
            comment_third_party: null,
            reinvoiced: "PAID",
            currency_Insurance: null,
            Invoice_number: null,
            date_of_reimbursement: "10/01/2023",
            reimbursed_amount: null,
            date_of_declaration: null,
            date_of_feedback: null,
            comment_Insurance: null,
            Indemnification_of_insurer: null,
            Indemnification_date: null,
            currency_indemnisation: null,
            deductible_charge_TAT: 5000,
            damage_caused_by: null,
            comment_nature_of_damage: null,
            TAT_name_persons: null,
            outsourcer_company_name: null,
            outsourcer_persons: null,
            thirdparty_company_name: null,
            thirdparty_Activity_comments: null,
            thirdparty_persons: null,
            incident_report: null,
            liability_letter: null,
            insurance_declaration: null,
            created_at: "05/18/2023 12:23",
            updated_at: "10/03/2023 11:26",
            major: 0,
            damage_description: null,
            type_of_equipment: {
              id: 9,
              name: "Trailers",
              categorie: "Equipment",
              created_at: "16/02/2023 16:10",
              updated_at: "16/02/2023 16:10",
            },
            brand: {
              id: 6,
              name: "FABRISEM",
              categorie: "Equipment",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
            estimate: [
              {
                id: 59,
                temporary_or_permanent: "Permanent",
                currency_estimate: "EUR",
                equipment_id: 68,
                container_id: null,
                automobile_id: null,
                vessel_id: null,
                created_at: "18/05/2023 12:23",
                updated_at: "18/05/2023 12:23",
                other_valuation: [
                  {
                    id: 58,
                    name: "Repair",
                    currency: "EUR",
                    value: 70,
                    taux_change: 1,
                    value_valuation: 70,
                    estimate_id: 59,
                    created_at: "2023-05-18T12:23:32.000000Z",
                    updated_at: "2023-05-18T12:23:32.000000Z",
                  },
                ],
              },
              {
                id: 60,
                temporary_or_permanent: "Permanent",
                currency_estimate: "EUR",
                equipment_id: 68,
                container_id: null,
                automobile_id: null,
                vessel_id: null,
                created_at: "18/05/2023 12:24",
                updated_at: "18/05/2023 12:24",
                other_valuation: [
                  {
                    id: 59,
                    name: "Repair",
                    currency: "EUR",
                    value: 55,
                    taux_change: 1,
                    value_valuation: 55,
                    estimate_id: 60,
                    created_at: "2023-05-18T12:24:10.000000Z",
                    updated_at: "2023-05-18T12:24:10.000000Z",
                  },
                ],
              },
              {
                id: 61,
                temporary_or_permanent: "Permanent",
                currency_estimate: "EUR",
                equipment_id: 68,
                container_id: null,
                automobile_id: null,
                vessel_id: null,
                created_at: "18/05/2023 12:25",
                updated_at: "18/05/2023 12:25",
                other_valuation: [
                  {
                    id: 60,
                    name: "Labor cost",
                    currency: "EUR",
                    value: 1.18,
                    taux_change: 1,
                    value_valuation: 1.18,
                    estimate_id: 61,
                    created_at: "2023-05-18T12:25:12.000000Z",
                    updated_at: "2023-05-18T12:25:12.000000Z",
                  },
                ],
              },
            ],
            nature_of_damage: {
              id: 2,
              name: "Glass breakage",
              categorie: "Equipment",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
            companie: null,
            department: {
              id: 4,
              name: "TECHNICAL",
              email: null,
              created_at: "16/02/2023 16:10",
              updated_at: "16/02/2023 16:10",
            },
            matricule: {
              id: 136,
              id_equipment: "27",
              id_automobile: null,
              matricule: "VV9FDT000LZ121067",
              equipment: "Trailers",
              brand: "FABRISEM",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
          },
          {
            id: 124,
            name: null,
            claim_id: 1,
            type_of_equipment_id: 2,
            brand_id: 8,
            companie_id: null,
            nature_of_damage_id: 129,
            department_id: 2,
            matricule_id: 10,
            concerned_internal_department: null,
            cause_damage: null,
            Liability_letter_number: null,
            amount: null,
            currency: null,
            comment_third_party: null,
            reinvoiced: "PAID",
            currency_Insurance: null,
            Invoice_number: null,
            date_of_reimbursement: "10/01/2023",
            reimbursed_amount: null,
            date_of_declaration: null,
            date_of_feedback: null,
            comment_Insurance: null,
            Indemnification_of_insurer: null,
            Indemnification_date: null,
            currency_indemnisation: null,
            deductible_charge_TAT: 5000,
            damage_caused_by: null,
            comment_nature_of_damage: null,
            TAT_name_persons: null,
            outsourcer_company_name: null,
            outsourcer_persons: null,
            thirdparty_company_name: null,
            thirdparty_Activity_comments: null,
            thirdparty_persons: null,
            incident_report: null,
            liability_letter: null,
            insurance_declaration: null,
            created_at: "09/06/2023 11:14",
            updated_at: "10/03/2023 11:26",
            major: 0,
            damage_description: null,
            type_of_equipment: {
              id: 2,
              name: "RTGs",
              categorie: "Equipment",
              created_at: "16/02/2023 16:10",
              updated_at: "16/02/2023 16:10",
            },
            brand: {
              id: 8,
              name: "SEACOM AG",
              categorie: "Equipment",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
            estimate: [],
            nature_of_damage: {
              id: 129,
              name: "TL 04 hit a unit",
              categorie: "equipment",
              created_at: "2023-05-02T11:38:21.000000Z",
              updated_at: "2023-05-02T11:38:21.000000Z",
            },
            companie: null,
            department: {
              id: 2,
              name: "OPERATIONS",
              email: null,
              created_at: "16/02/2023 16:10",
              updated_at: "16/02/2023 16:10",
            },
            matricule: {
              id: 10,
              id_equipment: "02",
              id_automobile: null,
              matricule: "G2440",
              equipment: "RTGs",
              brand: "KONECRANES",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
          },
          {
            id: 125,
            name: null,
            claim_id: 1,
            type_of_equipment_id: 2,
            brand_id: 8,
            companie_id: null,
            nature_of_damage_id: 129,
            department_id: 2,
            matricule_id: 10,
            concerned_internal_department: null,
            cause_damage: null,
            Liability_letter_number: null,
            amount: null,
            currency: null,
            comment_third_party: null,
            reinvoiced: "TO PROCEED",
            currency_Insurance: null,
            Invoice_number: null,
            date_of_reimbursement: "10/01/2023",
            reimbursed_amount: null,
            date_of_declaration: null,
            date_of_feedback: null,
            comment_Insurance: null,
            Indemnification_of_insurer: null,
            Indemnification_date: null,
            currency_indemnisation: null,
            deductible_charge_TAT: 5000,
            damage_caused_by: "TangerAlliance",
            comment_nature_of_damage: null,
            TAT_name_persons: "aaa ddd|aaz dsd|dss saaz",
            outsourcer_company_name: null,
            outsourcer_persons: null,
            thirdparty_company_name: null,
            thirdparty_Activity_comments: null,
            thirdparty_persons: null,
            incident_report: null,
            liability_letter: null,
            insurance_declaration: null,
            created_at: "09/06/2023 11:17",
            updated_at: "10/10/2023 11:34",
            major: 0,
            damage_description: null,
            type_of_equipment: {
              id: 2,
              name: "RTGs",
              categorie: "Equipment",
              created_at: "16/02/2023 16:10",
              updated_at: "16/02/2023 16:10",
            },
            brand: {
              id: 8,
              name: "SEACOM AG",
              categorie: "Equipment",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
            estimate: [],
            nature_of_damage: {
              id: 129,
              name: "TL 04 hit a unit",
              categorie: "equipment",
              created_at: "2023-05-02T11:38:21.000000Z",
              updated_at: "2023-05-02T11:38:21.000000Z",
            },
            companie: null,
            department: {
              id: 2,
              name: "OPERATIONS",
              email: null,
              created_at: "16/02/2023 16:10",
              updated_at: "16/02/2023 16:10",
            },
            matricule: {
              id: 10,
              id_equipment: "02",
              id_automobile: null,
              matricule: "G2440",
              equipment: "RTGs",
              brand: "KONECRANES",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
          },
          {
            id: 126,
            name: null,
            claim_id: 1,
            type_of_equipment_id: null,
            brand_id: null,
            companie_id: null,
            nature_of_damage_id: 165,
            department_id: null,
            matricule_id: null,
            concerned_internal_department: null,
            cause_damage: null,
            Liability_letter_number: null,
            amount: null,
            currency: null,
            comment_third_party: null,
            reinvoiced: "TO PROCEED",
            currency_Insurance: null,
            Invoice_number: null,
            date_of_reimbursement: "10/01/2023",
            reimbursed_amount: null,
            date_of_declaration: null,
            date_of_feedback: null,
            comment_Insurance: null,
            Indemnification_of_insurer: null,
            Indemnification_date: null,
            currency_indemnisation: null,
            deductible_charge_TAT: 5000,
            damage_caused_by: null,
            comment_nature_of_damage: null,
            TAT_name_persons: null,
            outsourcer_company_name: null,
            outsourcer_persons: null,
            thirdparty_company_name: null,
            thirdparty_Activity_comments: null,
            thirdparty_persons: null,
            incident_report: null,
            liability_letter: null,
            insurance_declaration: null,
            created_at: "09/06/2023 14:06",
            updated_at: "10/03/2023 11:35",
            major: 0,
            damage_description: null,
            type_of_equipment: null,
            brand: null,
            estimate: [],
            nature_of_damage: {
              id: 165,
              name: "TL 04 hit a units",
              categorie: "equipment",
              created_at: "2023-09-06T14:06:14.000000Z",
              updated_at: "2023-09-06T14:06:14.000000Z",
            },
            companie: null,
            department: null,
            matricule: null,
          },
        ],
        containers: [
          {
            id: 1,
            name: null,
            categorie_of_container: "EMPTY",
            claim_id: 1,
            type_of_equipment_id: null,
            companie_id: null,
            nature_of_damage_id: 7,
            department_id: null,
            shipping_line_id: 1,
            status: null,
            concerned_internal_department: "2",
            containerType: "Dry container (all purpose)",
            containerID: "ZCSU8929070",
            cause_damage: null,
            marchandise: "NO",
            marchandise_nature: null,
            marchandise_type: null,
            Liability_letter_number: null,
            amount: 515446,
            currency: "EUR",
            comment_third_party: "545",
            reinvoiced: "PAID",
            currency_Insurance: null,
            Invoice_number: null,
            date_of_reimbursement: "11/08/2023",
            reimbursed_amount: null,
            date_of_declaration: "11/14/2023",
            date_of_feedback: "11/14/2023",
            comment_Insurance: "dsdsds",
            Indemnification_of_insurer: "dsdsd",
            Indemnification_date: "2023-11-15",
            currency_indemnisation: "EUR",
            deductible_charge_TAT: 5454,
            damage_caused_by: null,
            comment_nature_of_damage: null,
            TAT_name_persons: null,
            outsourcer_company_name: null,
            outsourcer_persons: null,
            thirdparty_company_name: null,
            thirdparty_Activity_comments: null,
            thirdparty_persons: null,
            incident_report: null,
            liability_letter: null,
            insurance_declaration: null,
            created_at: "02/16/2023 16:52",
            updated_at: "11/13/2023 14:07",
            major: 0,
            damage_description: null,
            type_of_equipment: null,
            nature_of_damage: {
              id: 7,
              name: "Missing EIR",
              categorie: "container",
              created_at: "2023-02-16T16:52:20.000000Z",
              updated_at: "2023-02-16T16:52:20.000000Z",
            },
            companie: null,
            department: null,
            estimate: [
              {
                id: 161,
                temporary_or_permanent: "Temporary",
                currency_estimate: "EUR",
                equipment_id: null,
                container_id: 1,
                automobile_id: null,
                vessel_id: null,
                created_at: "13/11/2023 14:10",
                updated_at: "13/11/2023 14:10",
                other_valuation: [
                  {
                    id: 161,
                    name: "Transportation costs",
                    currency: "EUR",
                    value: 5635,
                    taux_change: 4,
                    value_valuation: 22540,
                    estimate_id: 161,
                    created_at: "2023-11-13T14:10:45.000000Z",
                    updated_at: "2023-11-13T14:10:45.000000Z",
                  },
                  {
                    id: 162,
                    name: "Costs of cargo",
                    currency: "EUR",
                    value: 45454,
                    taux_change: 4,
                    value_valuation: 181816,
                    estimate_id: 161,
                    created_at: "2023-11-13T14:10:45.000000Z",
                    updated_at: "2023-11-13T14:10:45.000000Z",
                  },
                ],
              },
            ],
            shipping_line: {
              id: 1,
              name: "ZIM",
              created_at: "2023-02-16T16:52:20.000000Z",
              updated_at: "2023-02-16T16:52:20.000000Z",
            },
          },
        ],
        vessels: [
          {
            id: 53,
            name: null,
            claim_id: 1,
            nature_of_damage_id: 45,
            companie_id: null,
            shipping_line_id: 2,
            concerned_internal_department: "3|4",
            vessel_number: "055298",
            cause_damage: "fghgfhg",
            Liability_letter_number: null,
            amount: 558,
            currency: "EUR",
            comment_third_party: "ikjhgfd",
            reinvoiced: "PAID",
            currency_Insurance: null,
            Invoice_number: null,
            date_of_reimbursement: "11/14/2023",
            reimbursed_amount: null,
            date_of_declaration: "11/14/2023",
            date_of_feedback: "11/14/2023",
            comment_Insurance: "zszsz",
            Indemnification_of_insurer: "54564",
            Indemnification_date: "11/14/2023",
            currency_indemnisation: "EUR",
            deductible_charge_TAT: 5000,
            damage_caused_by: "Thirdparty",
            comment_nature_of_damage: null,
            TAT_name_persons: null,
            outsourcer_company_name: null,
            outsourcer_persons: null,
            thirdparty_company_name: "ghjtyj",
            thirdparty_Activity_comments: "ytjytjytj",
            thirdparty_persons: "yyy|yyy",
            incident_report: null,
            liability_letter: null,
            insurance_declaration: null,
            created_at: "11/13/2023 14:14",
            updated_at: "11/13/2023 14:17",
            major: 0,
            damage_description: "gfhgfhf",
            type_of_equipment: null,
            companie: null,
            nature_of_damage: {
              id: 45,
              name: "Cell guide damaged",
              categorie: "vessel",
              created_at: "2023-02-21T08:58:25.000000Z",
              updated_at: "2023-02-21T08:58:25.000000Z",
            },
            department: null,
            estimate: [
              {
                id: 162,
                temporary_or_permanent: "Temporary",
                currency_estimate: "EUR",
                equipment_id: null,
                container_id: null,
                automobile_id: null,
                vessel_id: 53,
                created_at: "13/11/2023 14:16",
                updated_at: "13/11/2023 14:16",
                other_valuation: [
                  {
                    id: 163,
                    name: "Costs of cargo",
                    currency: "EUR",
                    value: 4141,
                    taux_change: 1,
                    value_valuation: 4141,
                    estimate_id: 162,
                    created_at: "2023-11-13T14:16:28.000000Z",
                    updated_at: "2023-11-13T14:16:28.000000Z",
                  },
                  {
                    id: 164,
                    name: "Transportation costs",
                    currency: "EUR",
                    value: 1,
                    taux_change: 558,
                    value_valuation: 558,
                    estimate_id: 162,
                    created_at: "2023-11-13T14:16:30.000000Z",
                    updated_at: "2023-11-13T14:16:30.000000Z",
                  },
                ],
              },
            ],
            shipping_line: {
              id: 2,
              name: "HLC",
              created_at: "2023-02-20T10:14:33.000000Z",
              updated_at: "2023-02-20T10:14:33.000000Z",
            },
          },
          {
            id: 54,
            name: null,
            claim_id: 1,
            nature_of_damage_id: 45,
            companie_id: null,
            shipping_line_id: 2,
            concerned_internal_department: "3|4",
            vessel_number: "055298",
            cause_damage: "fghgfhg",
            Liability_letter_number: null,
            amount: null,
            currency: null,
            comment_third_party: null,
            reinvoiced: null,
            currency_Insurance: null,
            Invoice_number: null,
            date_of_reimbursement: null,
            reimbursed_amount: null,
            date_of_declaration: null,
            date_of_feedback: null,
            comment_Insurance: null,
            Indemnification_of_insurer: null,
            Indemnification_date: null,
            currency_indemnisation: null,
            deductible_charge_TAT: 5000,
            damage_caused_by: "Thirdparty",
            comment_nature_of_damage: null,
            TAT_name_persons: null,
            outsourcer_company_name: null,
            outsourcer_persons: null,
            thirdparty_company_name: "ghjtyj",
            thirdparty_Activity_comments: "ytjytjytj",
            thirdparty_persons: "yyy|yyy",
            incident_report: null,
            liability_letter: null,
            insurance_declaration: null,
            created_at: "11/13/2023 14:14",
            updated_at: "11/13/2023 14:14",
            major: 1,
            damage_description: "gfhgfhf",
            type_of_equipment: null,
            companie: null,
            nature_of_damage: {
              id: 45,
              name: "Cell guide damaged",
              categorie: "vessel",
              created_at: "2023-02-21T08:58:25.000000Z",
              updated_at: "2023-02-21T08:58:25.000000Z",
            },
            department: null,
            estimate: [],
            shipping_line: {
              id: 2,
              name: "HLC",
              created_at: "2023-02-20T10:14:33.000000Z",
              updated_at: "2023-02-20T10:14:33.000000Z",
            },
          },
        ],
        automobiles: [
          {
            id: 1,
            name: null,
            claim_id: 1,
            type_of_equipment_id: 2,
            companie_id: null,
            brand_id: 8,
            nature_of_damage_id: 2,
            department_id: 2,
            matricule_id: 11,
            concerned_internal_department: "2|1|3",
            cause_damage: "szszs",
            Liability_letter_number: null,
            amount: null,
            currency: null,
            comment_third_party: null,
            reinvoiced: null,
            currency_Insurance: null,
            Invoice_number: null,
            date_of_reimbursement: null,
            reimbursed_amount: null,
            date_of_declaration: null,
            date_of_feedback: null,
            comment_Insurance: null,
            Indemnification_of_insurer: null,
            Indemnification_date: null,
            currency_indemnisation: null,
            deductible_charge_TAT: 5000,
            damage_caused_by: "Thirdparty",
            comment_nature_of_damage: null,
            TAT_name_persons: null,
            outsourcer_company_name: null,
            outsourcer_persons: null,
            thirdparty_company_name: "zszszsz",
            thirdparty_Activity_comments: "szszs",
            thirdparty_persons: "sss|sss",
            incident_report: null,
            liability_letter: null,
            insurance_declaration: null,
            created_at: "11/13/2023 09:13",
            updated_at: "11/13/2023 09:13",
            major: 0,
            damage_description: "ssasz",
            matricule: {
              id: 11,
              id_equipment: "03",
              id_automobile: null,
              matricule: "G2441",
              equipment: "RTGs",
              brand: "KONECRANES",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
            type_of_equipment: {
              id: 2,
              name: "RTGs",
              categorie: "Equipment",
              created_at: "16/02/2023 16:10",
              updated_at: "16/02/2023 16:10",
            },
            brand: {
              id: 8,
              name: "SEACOM AG",
              categorie: "Equipment",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
            nature_of_damage: {
              id: 2,
              name: "Glass breakage",
              categorie: "Equipment",
              created_at: "2023-02-16T16:10:44.000000Z",
              updated_at: "2023-02-16T16:10:44.000000Z",
            },
            companie: null,
            department: {
              id: 2,
              name: "OPERATIONS",
              email: null,
              created_at: "16/02/2023 16:10",
              updated_at: "16/02/2023 16:10",
            },
            estimate: [
              {
                id: 159,
                temporary_or_permanent: "Permanent",
                currency_estimate: "EUR",
                equipment_id: null,
                container_id: null,
                automobile_id: 1,
                vessel_id: null,
                created_at: "13/11/2023 09:14",
                updated_at: "13/11/2023 09:14",
                other_valuation: [
                  {
                    id: 159,
                    name: null,
                    currency: "EUR",
                    value: 125,
                    taux_change: 1,
                    value_valuation: 125,
                    estimate_id: 159,
                    created_at: "2023-11-13T09:14:41.000000Z",
                    updated_at: "2023-11-13T09:14:41.000000Z",
                  },
                  {
                    id: 160,
                    name: "Costs of repair",
                    currency: "EUR",
                    value: 588,
                    taux_change: 1,
                    value_valuation: 588,
                    estimate_id: 159,
                    created_at: "2023-11-13T09:14:41.000000Z",
                    updated_at: "2023-11-13T09:14:41.000000Z",
                  },
                ],
              },
            ],
          },
        ],
      },
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
      ],
      containerHeaders: [
        {
          text: "VESSEL NUMBER",
          sortable: true,
          value: "vessel_number",
        },
        {
          text: "SHIPPING LINE",
          value: "shipping_line.name",
          sortable: true,
        },
      ],
    };
  },

  mounted() {
    this.setDepartementsAction();
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
  },
  computed: {
    ...mapGetters(["getdepartements"]),
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
  height: 73px;
  padding-top: 26px;
  /* background: #1f497d; */
  font-weight: bold;
  /* color: white; */
  font-size: 25px;
  margin-bottom: 15px;
}
.equipHeader {
  margin: 0px auto;

  text-align: center;
  height: 65px;
  padding-top: 14px;
  background: #1f497d;
  font-weight: bold;
  color: white;
  letter-spacing: 6px;
  font-size: 18px;
}
.detailTitle {
  font-weight: bold;
}
</style>
