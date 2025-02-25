# Copyright (c) Facebook, Inc. and its affiliates.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

add_library(velox_type_signature OBJECT TypeSignature.cpp)

target_link_libraries(velox_type_signature velox_common_base)

add_library(
  velox_expression_functions FunctionSignature.cpp SignatureBinder.cpp
                             ReverseSignatureBinder.cpp)

target_link_libraries(
  velox_expression_functions velox_common_base velox_type_calculation
  velox_type_signature velox_signature_parser)

add_library(
  velox_expression
  BooleanMix.cpp
  CastExpr.cpp
  PrestoCastHooks.cpp
  CoalesceExpr.cpp
  ConjunctExpr.cpp
  ConstantExpr.cpp
  EvalCtx.cpp
  Expr.cpp
  ExprCompiler.cpp
  ExprToSubfieldFilter.cpp
  FieldReference.cpp
  FunctionCallToSpecialForm.cpp
  LambdaExpr.cpp
  VectorFunction.cpp
  RegisterSpecialForm.cpp
  RowConstructor.cpp
  SimpleFunctionRegistry.cpp
  SpecialFormRegistry.cpp
  SwitchExpr.cpp
  TryExpr.cpp
  GenericWriter.cpp
  PeeledEncoding.cpp)

target_link_libraries(
  velox_expression velox_core velox_vector velox_common_base
  velox_expression_functions velox_functions_util)

add_subdirectory(type_calculation)
add_subdirectory(signature_parser)

if(${VELOX_BUILD_TESTING})
  add_subdirectory(tests)
elseif(${VELOX_BUILD_TEST_UTILS})
  add_subdirectory(tests/utils)
endif()
