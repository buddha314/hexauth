<script>
  import { onMount } from 'svelte';
  import { Link } from 'svelte-routing';
  import 'bootstrap/dist/css/bootstrap.min.css';
    import { identity } from 'svelte/internal';

    import collapse from 'svelte-collapse';
    let open = true;
    let risk_reg_open = true;
    let geo_open = true;
    let logistic_open = true;
    let socio_open = true;

    // Put these together in order of the spreadsheet, from right to left
    //
    // SECTION ONE: RISK AND REGULATORY
    // // //
    let fed_local_tax_w =0.25;
    let fed_local_tax_s =1;
    let low_carbon_fuel_w =0.25;
    let low_carbon_fuel_s =1;
    let ag_advocay_orgs_w = 0.25;
    let ag_advocay_orgs_s = 1;
    let carbon_credits_w = 0.25;
    let carbon_credits_s = 1;
    // // 
    let reg_issues_bioeng_ca_w = 0.20;    
    let reg_issues_bioeng_ca_s = 1;    
    // // 
    let epa_risk_assess_data_w = 0.15;
    let epa_risk_assess_data_s = 1;
    // //
    let biomass_incentives_w = 0.50;
    $: biomass_incentives_s = (fed_local_tax_w * fed_local_tax_s 
        + low_carbon_fuel_w * low_carbon_fuel_s
        + ag_advocay_orgs_w * ag_advocay_orgs_s
        + carbon_credits_w * carbon_credits_s
        );
    // // 
    let food_security_w = .15;
    let food_security_s = 1;
    //
    let risk_regulatory_w = 0.25;
    $: risk_regulatory_s = (
        reg_issues_bioeng_ca_w * reg_issues_bioeng_ca_s
        + epa_risk_assess_data_w * epa_risk_assess_data_s
        + biomass_incentives_w * biomass_incentives_s
        + food_security_w * food_security_s
    );

    //
    // SECTION TWO: Geographical
    //
    // // //
    let rainfall_w = 0.25;
    let rainfall_s = 1;
    let temp_w = 0.25;
    let temp_s = 1;
    let growing_days_w = 0.25;
    let growing_days_s = 1;
    let climate_change_risk_w = 0.25;
    let climate_change_risk_s = 1;
    // // //
    let elevation_w = 0.25;
    let elevation_s = 1;
    let soil_conditions_w = 0.25;
    let soil_conditions_s = 1;
    let drought_w = 0.25;
    let drought_s = 1;
    let irrigation_w = 0.25;
    let irrigation_s = 1;
    // //
    let climatology_w = 0.50;
    $: climatology_s = (
        rainfall_w * rainfall_s
        + temp_w * temp_s
        + growing_days_w * growing_days_s
        + climate_change_risk_w * climate_change_risk_s
    )
    // //
    let physiographic_ecoregion_w = 0.50;
    $: physiographic_ecoregion_s = (
        elevation_w * elevation_s
        + soil_conditions_w * soil_conditions_s
        + drought_w * drought_s
        + irrigation_w * irrigation_s
    );
    //
    let geographical_w = 0.25;
    $: geographical_s = (
        climatology_w * climatology_s
        + physiographic_ecoregion_w * physiographic_ecoregion_s
    );

    //
    // SECTION THREE: LOGISTICAL DATA
    // 
    // // // //
    let surface_w = 0.30;
    let surface_s = 1;
    let ground_w = 0.30;
    let ground_s = 1;
    let logistic_rainfall_w = 0.40;
    let logistic_rainfall_s = 1;
    // // //
    let irrigation_type_cost_w = 0.35;
    $: irrigation_type_cost_s = (
        surface_w * surface_s
        + ground_w * ground_s
        + logistic_rainfall_w * logistic_rainfall_s
    );
    // // // //
    let crops_farmed_w = 0.33;
    let crops_farmed_s = 1;
    let acreage_trends_w = 0.33;
    let acreage_trends_s = 1;
    let econ_xano_vs_default_w = 0.33;
    let econ_xano_vs_default_s = 1;
    // // //
    let avail_land_usage_w = 0.40;
    $: avail_land_usage_s = (
        crops_farmed_w * crops_farmed_s
        + acreage_trends_w * acreage_trends_s
        + econ_xano_vs_default_w * econ_xano_vs_default_s
    );
    // // //
    let farming_equipment_w  = 0.15;
    let farming_equipment_s  = 1;
    let other_inputs_w = 0.10;
    let other_inputs_s = 1;
    // //
    let ag_infrastructure_w = 0.33;
    $: ag_infrastructure_s = (
        irrigation_type_cost_w * irrigation_type_cost_s
        + avail_land_usage_w * avail_land_usage_s
        + farming_equipment_w * farming_equipment_s
        + other_inputs_w * other_inputs_s
    );
    // // // //
    let cost_efficiency_w = 0.50;
    let cost_efficiency_s = 1;
    let labor_workforce_w = 0.20;
    let labor_workforce_s = 1;
    let seasonality_w = 0.30;
    let seasonality_s = 1;
    // // //
    let freight_mode_w = 0.40;
    $: freight_mode_s = (
        cost_efficiency_w * cost_efficiency_s
        + labor_workforce_w * labor_workforce_s
        + seasonality_w * seasonality_s
    );
    // // // //
    let distance_to_farm_w = 0.19;
    let distance_to_farm_s = 1;
    let freeway_radius_w = 0.43;
    let freeway_radius_s = 1;
    let distance_to_rail_w = 0.38;
    let distance_to_rail_s = 1;
    // // //
    let fixed_variables_w = 0.60;
    $: fixed_variables_s = (
        distance_to_farm_w * distance_to_farm_s
        + freeway_radius_w * freeway_radius_s
        + distance_to_rail_w * distance_to_rail_s
    );
    // //
    let transportation_infrastructure_w = 0.34;
    $: transportation_infrastructure_s = (
        freight_mode_w * freight_mode_s
        + fixed_variables_w * fixed_variables_s
    );
    // // // //
    let bioeng_infra_operational_w = 0.70;
    let bioeng_infra_operational_s = 1;
    let active_dev_w = 0.25;
    let active_dev_s = 1;
    let idle_closed_w = 0.05;
    let idle_closed_s = 1;
    // // //
    let operating_status_w = 0.55;
    $: operating_status_s = (
        bioeng_infra_operational_w * bioeng_infra_operational_s
        + active_dev_w * active_dev_s
        + idle_closed_w * idle_closed_s
    );
    // // // //
    let close_highway_fwy_w = 0.75;
    let close_highway_fwy_s = 1;
    let close_to_rail_port_w = 0.25;
    let close_to_rail_port_s = 1;
    // // //
    let location_access_w = 0.05;
    $: location_access_s = (
        close_highway_fwy_w * close_highway_fwy_s
        + close_to_rail_port_w * close_to_rail_port_s
    );
    let bioeng_materials_w = 0.05;
    let bioeng_materials_s = 1;
    // // 
    let bioeng_production_infra_w = 0.33;
    let bioeng_production_infra_s = (
        operating_status_w * operating_status_s
        + location_access_w * location_access_s
        + bioeng_materials_w * bioeng_materials_s
    );
    //
    let logistical_data_w = 0.25;
    $: logistical_data_s = (
        ag_infrastructure_w * ag_infrastructure_s
        + transportation_infrastructure_w * transportation_infrastructure_s
        + bioeng_production_infra_w * bioeng_production_infra_s
    );
    //
    // SECTION FOUR: SOCIOECONOMIC DATA
    //
    // //
    let opportunity_zone_w = 0.40;
    let opportunity_zone_s = 1;
    let workforce_w = 0.35;
    let workforce_s = 1;
    let ag_education_w = 0.25;
    let ag_education_s = 1;
    // 
    let socioeconomic_w = 0.25;
    let socioeconomic_s = (
        opportunity_zone_w * opportunity_zone_s
        + workforce_w * workforce_s
        + ag_education_w * ag_education_s
    );

    $: total_score_s = (
        risk_regulatory_w * risk_regulatory_s
        + geographical_w * geographical_s
        + logistical_data_w * logistical_data_s
        + socioeconomic_w * socioeconomic_s
    ) * 100;

</script>

    <div class="section-header">Risk & Regulatory</div>
    <div class="row" on:click={() => risk_reg_open = !risk_reg_open}>
        <div class="col-10 desc">Risk & Regulatory</div>
        <div class="col-1 weight">{risk_regulatory_w}</div>
        <div class="col-1 score">{risk_regulatory_s}</div>
    </div>
    <div class="row" use:collapse={{open: risk_reg_open}}>
        <div class="col-1 blank"></div>
        <div class="col-7 desc">Regulatory issues relating to bioenergy in CA</div>
        <div class="col-1 weight"><input bind:value={reg_issues_bioeng_ca_w}/></div> 
        <div class="col-1 weight"><input bind:value={reg_issues_bioeng_ca_s}/></div>
        <div class="col-2 blank"></div>
    </div>
    <div class="row" use:collapse={{open: risk_reg_open}}>
        <div class="col-1 blank"></div>
        <div class="col-7 desc">EPA or DOE risk assessment data</div>
        <div class="col-1 weight"><input bind:value={epa_risk_assess_data_w}/></div>
        <div class="col-1 weight"><input bind:value={epa_risk_assess_data_s}/></div>
        <div class="col-2 blank"></div>
    </div>
    <div class="row" use:collapse={{open: risk_reg_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Federal & Local Tax Incentives</div>
        <div class="col-1 weight"><input bind:value={fed_local_tax_w}/></div>
        <div class="col-1 weight"><input bind:value={fed_local_tax_s}/></div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: risk_reg_open}}>
        <div class="col-1 blank"></div>
        <td class="col-5 desc">Low Carbon Fuel Standards</td>
        <td class="col-1 weight"><input bind:value={low_carbon_fuel_w}/></td>
        <td class="col-1 weight"><input bind:value={low_carbon_fuel_s}/></td>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: risk_reg_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Agriculture Advocacy Organizations</div>
        <div class="col-1 weight"><input bind:value={ag_advocay_orgs_w}/></div>
        <div class="col-1 weight"><input bind:value={ag_advocay_orgs_s}/></div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: risk_reg_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Carbon Credits</div>
        <div class="col-1 weight"><input bind:value={carbon_credits_w}/></div>
        <div class="col-1 weight"><input bind:value={carbon_credits_s}/></div>
        <div class="col-4 blank"></div>
    </div>

    <div class="row" use:collapse={{open: risk_reg_open}}>
        <div class="col-1 blank"></div>
        <div class="col-7 desc" >Biomass incentives - State & Federal</div>
        <div class="col-1 weight" ><input bind:value={biomass_incentives_w}/></div>
        <div class="col-1 score" >{biomass_incentives_s}</div>
        <div class="col-2 blank"></div>
    </div>
    <div class="row" use:collapse={{open: risk_reg_open}}>
        <div class="col-1 blank"></div>
        <div class="col-7 desc">Food Security</div>
        <div class="col-1 weight"><input bind:value={food_security_w}/></div>
        <div class="col-1 weight"><input bind:value={food_security_s}/></div>
        <div class="col-2 blank"></div>
    </div>


    <div class="section-header" colspan="12">Geographical</div>
    <div class="row" on:click={() => geo_open = !geo_open}>
        <div class="col-10 desc" >Geographic</div>
        <div class="col-1 weight" ><input bind:value={geographical_w}/></div>
        <div class="col-1 score" >{geographical_s}</div>
    </div>
    <div class="row" use:collapse={{open: geo_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Rainfall</div>
        <div class="col-1 weight"><input bind:value={rainfall_w}/></div>
        <div class="col-1 weight"><input bind:value={rainfall_s}/></div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: geo_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Temperature</div>
        <div class="col-1 weight"><input bind:value={temp_w}/></div>
        <div class="col-1 weight"><input bind:value={temp_s}/></div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: geo_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Growning Days</div>
        <div class="col-1 weight"><input bind:value={growing_days_w}/></div>
        <div class="col-1 weight"><input bind:value={growing_days_s}/></div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: geo_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Climate Change Risk</div>
        <div class="col-1 weight"><input bind:value={climate_change_risk_w}/></div>
        <div class="col-1 weight"><input bind:value={climate_change_risk_s}/></div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: geo_open}}>
        <div class="col-1 blank"></div>
        <div class="col-7 desc">Climatology</div>
        <div class="col-1 weight"><input bind:value={climatology_s}/></div>
        <div class="col-1 score">{climatology_w}</div>
        <div class="col-2 blank"></div>
    </div>
    <div class="row" use:collapse={{open: geo_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Elevation</div>
        <div class="col-1 weight"><input bind:value={elevation_w}/></div>
        <div class="col-1 weight"><input bind:value={elevation_s}/></div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: geo_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Soil Condition</div>
        <div class="col-1 weight"><input bind:value={soil_conditions_w}/></div>
        <div class="col-1 weight"><input bind:value={soil_conditions_s}/></div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: geo_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Drought</div>
        <div class="col-1 weight"><input bind:value={drought_w}/></div>
        <div class="col-1 weight"><input bind:value={drought_s}/></div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: geo_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Irrigation</div>
        <div class="col-1 weight"><input bind:value={irrigation_w}/></div>
        <div class="col-1 weight"><input bind:value={irrigation_s}/></div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: geo_open}}>
        <div class="col-1 blank"></div>
        <div class="col-7 desc">Physiographic Ecoregions</div>
        <div class="col-1 weight"><input bind:value={physiographic_ecoregion_w}/></div>
        <div class="col-1 score">{physiographic_ecoregion_s}</div>
        <div class="col-2 blank"></div>
    </div>


    <div class="section-header" colspan="12">Logistical Data</div>
    <div class="row" on:click={() => logistic_open = !logistic_open}>
        <div class="col-10 desc">Logistical Data</div>
        <div class="col-1 weight"><input bind:value={logistic_rainfall_w}/></div>
        <div class="col-1 score">{logistical_data_w}</div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Surface</div>
        <div class="col-1 weight"><input bind:value={surface_w}/></div>
        <div class="col-1 weight"><input bind:value={surface_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Ground</div>
        <div class="col-1 weight"><input bind:value={ground_w}/></div>
        <div class="col-1 weight"><input bind:value={ground_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Rainfall</div>
        <div class="col-1 weight"><input bind:value={rainfall_w}/></div>
        <div class="col-1 weight"><input bind:value={rainfall_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Irrigation - Type & Cost</div>
        <div class="col-1 weight"><input bind:value={irrigation_type_cost_w}/></div>
        <div class="col-1 score">{irrigation_type_cost_s}</div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Crops Farmed</div>
        <div class="col-1 weight"><input bind:value={crops_farmed_w}/></div>
        <div class="col-1 weight"><input bind:value={crops_farmed_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Acreage Trends</div>
        <div class="col-1 weight"><input bind:value={acreage_trends_w}/></div>
        <div class="col-1 weight"><input bind:value={acreage_trends_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Compare Economics of Xano Grass vs Competing Land Use(s)</div>
        <div class="col-1 weight"><input bind:value={econ_xano_vs_default_w}/></div>
        <div class="col-1 weight"><input bind:value={econ_xano_vs_default_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Available Land & Usage</div>
        <div class="col-1 weight"><input bind:value={avail_land_usage_w}/></div>
        <div class="col-1 score">{avail_land_usage_s}</div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Farming Equipment</div>
        <div class="col-1 weight"><input bind:value={farming_equipment_w}/></div>
        <div class="col-1 weight"><input bind:value={farming_equipment_s}/></div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Other Inputs</div>
        <div class="col-1 weight"><input bind:value={other_inputs_w}/></div>
        <div class="col-1 weight"><input bind:value={other_inputs_s}/></div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-7 desc">Agricultural Infrastructure</div>
        <div class="col-1 weight"><input bind:value={ag_infrastructure_w}/></div>
        <div class="col-1 score">{ag_infrastructure_s}</div>
        <div class="col-2 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Cost & Efficiency</div>
        <div class="col-1 weight"><input bind:value={cost_efficiency_w}/></div>
        <div class="col-1 weight"><input bind:value={cost_efficiency_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Labor & Efficiency</div>
        <div class="col-1 weight"><input bind:value={labor_workforce_w}/></div>
        <div class="col-1 weight"><input bind:value={labor_workforce_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Seasonality</div>
        <div class="col-1 weight"><input bind:value={seasonality_w}/></div>
        <div class="col-1 weight"><input bind:value={seasonality_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Freight Mode Selection</div>
        <div class="col-1 weight"><input bind:value={freight_mode_w}/></div>
        <div class="col-1 score">{freight_mode_s}</div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Distance to Farm</div>
        <div class="col-1 weight"><input bind:value={distance_to_farm_w}/></div>
        <div class="col-1 weight"><input bind:value={distance_to_farm_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">60 Mile Radius Inclusive of Freeway/Highway</div>
        <div class="col-1 weight"><input bind:value={freeway_radius_w}/></div>
        <div class="col-1 weight"><input bind:value={freeway_radius_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Farm Distance to Rail</div>
        <div class="col-1 weight"><input bind:value={distance_to_rail_w}/></div>
        <div class="col-1 weight"><input bind:value={distance_to_farm_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Fixed Variables</div>
        <div class="col-1 weight"><input bind:value={fixed_variables_w}/></div>
        <div class="col-1 score">{fixed_variables_s}</div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-7 desc">Transportation Infrastructure</div>
        <div class="col-1 weight"><input bind:value={transportation_infrastructure_w}/></div>
        <div class="col-1 score">{transportation_infrastructure_s}</div>
        <div class="col-2 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Operational</div>
        <div class="col-1 weight"><input bind:value={bioeng_infra_operational_w}/></div>
        <div class="col-1 weight"><input bind:value={bioeng_infra_operational_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Active Development, Proposed</div>
        <div class="col-1 weight"><input bind:value={active_dev_w}/></div>
        <div class="col-1 weight"><input bind:value={active_dev_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Idle, Closed</div>
        <div class="col-1 weight"><input bind:value={idle_closed_w}/></div>
        <div class="col-1 weight"><input bind:value={idle_closed_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Operating Status</div>
        <div class="col-1 weight"><input bind:value={operating_status_w}/></div>
        <div class="col-1 score">{operating_status_s}</div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Close to Highways/Interstates/Freeways?</div>
        <div class="col-1 weight"><input bind:value={close_highway_fwy_w}/></div>
        <div class="col-1 weight"><input bind:value={close_highway_fwy_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-3 desc">Close to Railways/Ports?</div>
        <div class="col-1 weight"><input bind:value={close_to_rail_port_w}/></div>
        <div class="col-1 weight"><input bind:value={close_to_rail_port_s}/></div>
        <div class="col-6 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc">Location/Access</div>
        <div class="col-1 weight"><input bind:value={location_access_w}/></div>
        <div class="col-1 score">{location_access_s}</div>
        <div class="col-4 blank"></div>
    </div>
    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-5 desc"> Inputs/Outputs (Lignocellulosic to Electricity)</div>
        <div class="col-1 weight"><input bind:value={bioeng_materials_w}/></div>
        <div class="col-1 weight"><input bind:value={bioeng_materials_s}/></div>
        <div class="col-4 blank"></div>
    </div>

    <div class="row" use:collapse={{open: logistic_open}}>
        <div class="col-1 blank"></div>
        <div class="col-7 desc">Bioenergy Production Infrastructure</div>
        <div class="col-1 weight"><input bind:value={bioeng_production_infra_w}/></div>
        <div class="col-1 score">{bioeng_infra_operational_s}</div>
        <div class="col-2 blank"></div>
    </div>

    <div class="section-header" colspan="12">Socioeconomic Data</div>
    <div class="row" on:click={() => socio_open= !socio_open}>
        <div class="col-10 desc">Socioeconomic Data</div>
        <div class="col-1 weight"><input bind:value={socioeconomic_w}/></div>
        <div class="col-1 score">{socioeconomic_s}</div>
    </div>
    <div class="row" use:collapse={{open: socio_open}}>
        <div class="col-1 blank"></div>
        <div class="col-7 desc">Opportunity Zones</div>
        <div class="col-1 weight"><input bind:value={opportunity_zone_w}/></div>
        <div class="col-1 weight"><input bind:value={opportunity_zone_s}/></div>
        <div class="col-2 blank"></div>
    </div>
    <div class="row" use:collapse={{open: socio_open}}>
        <div class="col-1 blank"></div>
        <div class="col-7 desc">Workforce / Labor</div>
        <div class="col-1 weight"><input bind:value={workforce_w}/></div>
        <div class="col-1 weight"><input bind:value={workforce_s}/></div>
        <div class="col-2 blank"></div>
    </div>
    <div class="row" use:collapse={{open: socio_open}}>
        <div class="col-1 blank"></div>
        <div class="col-7 desc">Agricultural Education Support</div>
        <div class="col-1 weight"><input bind:value={ag_education_w}/></div>
        <div class="col-1 weight"><input bind:value={ag_education_s}/></div>
        <div class="col-2 blank"></div>
    </div>



<style>
.weight {
    background-color: yellow;
    vertical-align: middle;
}
.score {
    background-color: palegreen;
    vertical-align: middle;
}
.desc {
    background-color: lightgrey;
    vertical-align: middle;
}
.blank {
    background-color: pink;
}
.section-header {
    font-size: 30px;
    text-align: center;
}
input {
    width: 50px;
    border: none;
}
</style>